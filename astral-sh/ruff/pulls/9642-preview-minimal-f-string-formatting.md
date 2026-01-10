```yaml
number: 9642
title: Preview minimal f-string formatting
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/fstring-formatting
created_at: 2024-01-25T14:25:39Z
updated_at: 2024-02-16T14:58:32Z
url: https://github.com/astral-sh/ruff/pull/9642
synced_at: 2026-01-10T22:57:09Z
```

# Preview minimal f-string formatting

---

_Pull request opened by @dhruvmanila on 2024-01-25 14:25_

## Summary

_This is preview only feature and is available using the `--preview` command-line flag._

With the implementation of [PEP 701] in Python 3.12, f-strings can now be broken into multiple lines, can contain comments, and can re-use the same quote character. Currently, no other Python formatter formats the f-strings so there's some discussion which needs to happen in defining the style used for f-string formatting. Relevant discussion: https://github.com/astral-sh/ruff/discussions/9785

The goal for this PR is to add minimal support for f-string formatting. This would be to format expression within the replacement field without introducing any major style changes.

### Newlines

The heuristics for adding newline is similar to that of [Prettier](https://prettier.io/docs/en/next/rationale.html#template-literals) where the formatter would only split an expression in the replacement field across multiple lines if there was already a line break within the replacement field.

In other words, the formatter would not add any newlines unless they were already present i.e., they were added by the user. This makes breaking any expression inside an f-string optional and in control of the user. For example,

```python
# We wouldn't break this
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa { aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc } cccccccccc"

# But, we would break the following as there's already a newline
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa {
	aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc } cccccccccc"
```


If there are comments in any of the replacement field of the f-string, then it will always be a multi-line f-string in which case the formatter would prefer to break expressions i.e., introduce newlines. For example,

```python
x = f"{ # comment
    a }"
```

### Quotes

The logic for formatting quotes remains unchanged. The existing logic is used to determine the necessary quote char and is used accordingly.

Now, if the expression inside an f-string is itself a string like, then we need to make sure to preserve the existing quote and not change it to the preferred quote unless it's 3.12. For example,

```python
f"outer {'inner'} outer"

# For pre 3.12, preserve the single quote
f"outer {'inner'} outer"

# While for 3.12 and later, the quotes can be changed
f"outer {"inner"} outer"
```

But, for triple-quoted strings, we can re-use the same quote char unless the inner string is itself a triple-quoted string.

```python
f"""outer {"inner"} outer"""  # valid
f"""outer {'''inner'''} outer"""  # preserve the single quote char for the inner string
```

### Debug expressions

If debug expressions are present in the replacement field of a f-string, then the whitespace needs to be preserved as they will be rendered as it is (for example, `f"{  x  = }"`. If there are any nested f-strings, then the whitespace in them needs to be preserved as well which means that we'll stop formatting the f-string as soon as we encounter a debug expression.

```python
f"outer {   x =  !s  :.3f}"
#                  ^^
#                  We can remove these whitespaces
```

Now, the whitespace doesn't need to be preserved around conversion spec and format specifiers, so we'll format them as usual but we won't be formatting any nested f-string within the format specifier.

### Miscellaneous

- The [`hug_parens_with_braces_and_square_brackets`](https://github.com/astral-sh/ruff/issues/8279) preview style isn't implemented w.r.t. the f-string curly braces.
- The [indentation](https://github.com/astral-sh/ruff/discussions/9785#discussioncomment-8470590) is always relative to the f-string containing statement

## Test Plan

* Add new test cases
* Review existing snapshot changes
* Review the ecosystem changes

[PEP 701]: https://peps.python.org/pep-0701/


---

_Label `formatter` added by @dhruvmanila on 2024-02-12 14:27_

---

_Label `preview` added by @dhruvmanila on 2024-02-12 14:27_

---

_Renamed from "WIP: F-string formatting" to "Preview minimal f-string formatting" by @dhruvmanila on 2024-02-12 14:32_

---

_Closed by @dhruvmanila on 2024-02-13 07:40_

---

_Reopened by @dhruvmanila on 2024-02-13 07:40_

---

_Comment by @github-actions[bot] on 2024-02-13 08:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (error)</summary>
<p>

```
Failed to clone zulip/zulip: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 6838 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+483 -453 lines in 203 files in 22 projects; 21 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+3 -3 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/train.py#L43'>rasa/core/train.py~L43</a>
```diff
                     domain,
                     policy_config,
                     stories=story_file,
-                    output=str(Path(output_path, f"run_{r +1}")),
+                    output=str(Path(output_path, f"run_{r + 1}")),
                     fixed_model_name=config_name + PERCENTAGE_KEY + str(percentage),
                     additional_arguments={
                         **additional_arguments,
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/graph_components/validators/test_default_recipe_validator.py#L861'>tests/graph_components/validators/test_default_recipe_validator.py~L861</a>
```diff
 ):
     assert (
         len(policy_types) >= priority + num_duplicates
-    ), f"This tests needs at least {priority+num_duplicates} many types."
+    ), f"This tests needs at least {priority + num_duplicates} many types."
 
     # start with a schema where node i has priority i
     nodes = {
```
<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/graph_components/validators/test_default_recipe_validator.py#L871'>tests/graph_components/validators/test_default_recipe_validator.py~L871</a>
```diff
 
     # give nodes p+1, .., p+num_duplicates-1 priority "priority"
     for idx in range(num_duplicates):
-        nodes[f"{priority+idx+1}"].config["priority"] = priority
+        nodes[f"{priority + idx + 1}"].config["priority"] = priority
 
     validator = DefaultV1RecipeValidator(graph_schema=GraphSchema(nodes))
     monkeypatch.setattr(
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+20 -20 lines across 10 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/airflow/dag_processing/processor.py#L546'>airflow/dag_processing/processor.py~L546</a>
```diff
             <pre><code>{task_list}\n<code></pre>
             Blocking tasks:
             <pre><code>{blocking_task_list}<code></pre>
-            Airflow Webserver URL: {conf.get(section='webserver', key='base_url')}
+            Airflow Webserver URL: {conf.get(section="webserver", key="base_url")}
             """
 
             tasks_missed_sla = []
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/airflow/providers/google/cloud/hooks/dataproc_metastore.py#L685'>airflow/providers/google/cloud/hooks/dataproc_metastore.py~L685</a>
```diff
                     TBLS.TBL_NAME = '{table}'"""
         if _partitions:
             query += f"""
-                    AND PARTITIONS.PART_NAME IN ({', '.join(f"'{p}'" for p in _partitions)})"""
+                    AND PARTITIONS.PART_NAME IN ({", ".join(f"'{p}'" for p in _partitions)})"""
         query += ";"
 
         client = self.get_dataproc_metastore_client_v1beta()
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/airflow/providers/google/cloud/triggers/cloud_storage_transfer_service.py#L48'>airflow/providers/google/cloud/triggers/cloud_storage_transfer_service.py~L48</a>
```diff
     def serialize(self) -> tuple[str, dict[str, Any]]:
         """Serialize StorageTransferJobsTrigger arguments and classpath."""
         return (
-            f"{self.__class__.__module__ }.{self.__class__.__qualname__}",
+            f"{self.__class__.__module__}.{self.__class__.__qualname__}",
             {
                 "project_id": self.project_id,
                 "job_names": self.job_names,
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/dev/breeze/src/airflow_breeze/utils/packages.py#L338'>dev/breeze/src/airflow_breeze/utils/packages.py~L338</a>
```diff
     processed_package_filters.extend(get_long_package_names(short_packages))
 
     removed_packages: list[str] = [
-        f"apache-airflow-providers-{provider.replace('.','-')}" for provider in get_removed_provider_ids()
+        f"apache-airflow-providers-{provider.replace('.', '-')}" for provider in get_removed_provider_ids()
     ]
     all_packages_including_removed: list[str] = available_doc_packages + removed_packages
     invalid_filters = [
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/dev/breeze/src/airflow_breeze/utils/packages.py#L524'>dev/breeze/src/airflow_breeze/utils/packages.py~L524</a>
```diff
     prefix = "apache-airflow-providers-"
     base_url = "https://airflow.apache.org/docs/"
     for dependency in cross_package_dependencies:
-        pip_package_name = f"{prefix}{dependency.replace('.','-')}"
-        url_suffix = f"{dependency.replace('.','-')}"
+        pip_package_name = f"{prefix}{dependency.replace('.', '-')}"
+        url_suffix = f"{dependency.replace('.', '-')}"
         if markdown:
             url = f"[{pip_package_name}]({base_url}{url_suffix})"
         else:
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/dev/prepare_bulk_issues.py#L228'>dev/prepare_bulk_issues.py~L228</a>
```diff
             except GithubException as e:
                 console.print(f"[red]Error!: {e}[/]")
                 console.print(
-                    f"[yellow]Restart with `--start-from {processed_issues+start_from}` to continue.[/]"
+                    f"[yellow]Restart with `--start-from {processed_issues + start_from}` to continue.[/]"
                 )
         console.print(f"Created {processed_issues} issue(s).")
 
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/docker_tests/test_prod_image.py#L37'>docker_tests/test_prod_image.py~L37</a>
```diff
 PROD_IMAGE_PROVIDERS_FILE_PATH = SOURCE_ROOT / "prod_image_installed_providers.txt"
 AIRFLOW_ROOT_PATH = Path(__file__).parents[2].resolve()
 SLIM_IMAGE_PROVIDERS = [
-    f"apache-airflow-providers-{provider_id.replace('.','-')}"
+    f"apache-airflow-providers-{provider_id.replace('.', '-')}"
     for provider_id in AIRFLOW_PRE_INSTALLED_PROVIDERS_FILE_PATH.read_text().splitlines()
     if not provider_id.startswith("#")
 ]
 REGULAR_IMAGE_PROVIDERS = [
-    f"apache-airflow-providers-{provider_id.replace('.','-')}"
+    f"apache-airflow-providers-{provider_id.replace('.', '-')}"
     for provider_id in PROD_IMAGE_PROVIDERS_FILE_PATH.read_text().splitlines()
     if not provider_id.startswith("#")
 ]
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/jobs/test_scheduler_job.py#L4210'>tests/jobs/test_scheduler_job.py~L4210</a>
```diff
         with dag_maker("test_dagrun_states_are_correct_2", start_date=date) as dag:
             EmptyOperator(task_id="dummy_task")
         for i in range(16):
-            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i+1}", state=State.RUNNING, execution_date=date)
+            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i + 1}", state=State.RUNNING, execution_date=date)
             date = dr.execution_date + timedelta(hours=1)
         dr16 = DagRun.find(run_id="dr2_run_16")
         date = dr16[0].execution_date + timedelta(hours=1)
         for i in range(16, 32):
-            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i+1}", state=State.QUEUED, execution_date=date)
+            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i + 1}", state=State.QUEUED, execution_date=date)
             date = dr.execution_date + timedelta(hours=1)
 
         # third dag and dagruns
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/jobs/test_scheduler_job.py#L4223'>tests/jobs/test_scheduler_job.py~L4223</a>
```diff
         with dag_maker("test_dagrun_states_are_correct_3", start_date=date) as dag:
             EmptyOperator(task_id="dummy_task")
         for i in range(16):
-            dr = dag_maker.create_dagrun(run_id=f"dr3_run_{i+1}", state=State.RUNNING, execution_date=date)
+            dr = dag_maker.create_dagrun(run_id=f"dr3_run_{i + 1}", state=State.RUNNING, execution_date=date)
             date = dr.execution_date + timedelta(hours=1)
         dr16 = DagRun.find(run_id="dr3_run_16")
         date = dr16[0].execution_date + timedelta(hours=1)
         for i in range(16, 32):
-            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i+1}", state=State.QUEUED, execution_date=date)
+            dr = dag_maker.create_dagrun(run_id=f"dr2_run_{i + 1}", state=State.QUEUED, execution_date=date)
             date = dr.execution_date + timedelta(hours=1)
 
         scheduler_job = Job()
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/providers/amazon/aws/log/test_cloudwatch_task_handler.py#L117'>tests/providers/amazon/aws/log/test_cloudwatch_task_handler.py~L117</a>
```diff
             {"timestamp": current_time, "message": "Third"},
         ]
         assert [handler._event_to_str(event) for event in events] == ([
-            f"[{get_time_str(current_time-2000)}] First",
-            f"[{get_time_str(current_time-1000)}] Second",
+            f"[{get_time_str(current_time - 2000)}] First",
+            f"[{get_time_str(current_time - 1000)}] Second",
             f"[{get_time_str(current_time)}] Third",
         ])
 
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/providers/amazon/aws/log/test_cloudwatch_task_handler.py#L141'>tests/providers/amazon/aws/log/test_cloudwatch_task_handler.py~L141</a>
```diff
 
         msg_template = "*** Reading remote log from Cloudwatch log_group: {} log_stream: {}.\n{}\n"
         events = "\n".join([
-            f"[{get_time_str(current_time-2000)}] First",
-            f"[{get_time_str(current_time-1000)}] Second",
+            f"[{get_time_str(current_time - 2000)}] First",
+            f"[{get_time_str(current_time - 1000)}] Second",
             f"[{get_time_str(current_time)}] Third",
         ])
         assert self.cloudwatch_task_handler.read(self.ti) == (
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/system/providers/amazon/aws/example_sagemaker.py#L138'>tests/system/providers/amazon/aws/example_sagemaker.py~L138</a>
```diff
         dockerfile.write(
             f"""
             FROM public.ecr.aws/amazonlinux/amazonlinux
-            COPY {preprocessing_script.name.split('/')[2]} /preprocessing.py
+            COPY {preprocessing_script.name.split("/")[2]} /preprocessing.py
             ADD credentials /credentials
             ENV AWS_SHARED_CREDENTIALS_FILE=/credentials
             RUN yum install python3 pip -y
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/system/providers/amazon/aws/example_sagemaker.py#L182'>tests/system/providers/amazon/aws/example_sagemaker.py~L182</a>
```diff
     """generates a very simple csv dataset with headers"""
     content = "class,x,y\n"  # headers
     for i in range(SAMPLE_SIZE):
-        content += f"{i%100},{i},{SAMPLE_SIZE-i}\n"
+        content += f"{i % 100},{i},{SAMPLE_SIZE - i}\n"
     return content
 
 
```
<a href='https://github.com/apache/airflow/blob/a3e939f22f0e895d7b0897b4eba13908822f267d/tests/utils/log/test_secrets_masker.py#L211'>tests/utils/log/test_secrets_masker.py~L211</a>
```diff
             The above exception was the direct cause of the following exception:
 
             Traceback (most recent call last):
-              File ".../test_secrets_masker.py", line {line+4}, in test_masking_in_explicit_context_exceptions
+              File ".../test_secrets_masker.py", line {line + 4}, in test_masking_in_explicit_context_exceptions
                 raise RuntimeError(f"Exception: {{exception}}") from exception
             RuntimeError: Exception: Cannot connect to user:***
             """
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+31 -31 lines across 20 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/init/interactive_init_flow.py#L484'>samcli/commands/init/interactive_init_flow.py~L484</a>
```diff
     click.echo(f"\n{question}")
 
     for index, option in enumerate(options_list):
-        click.echo(f"\t{index+1} - {option}")
+        click.echo(f"\t{index + 1} - {option}")
         click_choices.append(str(index + 1))
     choice = click.prompt(msg, type=click.Choice(click_choices), show_choices=False)
     return options_list[int(choice) - 1]
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/init/interactive_init_flow.py#L497'>samcli/commands/init/interactive_init_flow.py~L497</a>
```diff
         click.echo("\nSelect your starter template")
         click_template_choices = []
         for index, template in enumerate(templates):
-            click.echo(f"\t{index+1} - {template['displayName']}")
+            click.echo(f"\t{index + 1} - {template['displayName']}")
             click_template_choices.append(str(index + 1))
         template_choice = click.prompt("Template", type=click.Choice(click_template_choices), show_choices=False)
         chosen_template = templates[int(template_choice) - 1]
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/local/invoke/core/command.py#L58'>samcli/commands/local/invoke/core/command.py~L58</a>
```diff
                     ),
                     RowDefinition(
                         name=style(
-                            f"$ echo {json.dumps({'message':'hello!'})} | "
+                            f"$ echo {json.dumps({'message': 'hello!'})} | "
                             f"{ctx.command_path} HelloWorldFunction -e -"
                         ),
                         extra_row_modifiers=[ShowcaseRowModifier()],
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/invoke/core/command.py#L38'>samcli/commands/remote/invoke/core/command.py~L38</a>
```diff
                         RowDefinition(
                             name=style(
                                 f"${ctx.command_path} --stack-name hello-world -e"
-                                f" '{json.dumps({'message':'hello!'})}'"
+                                f" '{json.dumps({'message': 'hello!'})}'"
                             ),
                             extra_row_modifiers=[ShowcaseRowModifier()],
                         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/invoke/core/command.py#L59'>samcli/commands/remote/invoke/core/command.py~L59</a>
```diff
                     formatter.write_rd([
                         RowDefinition(
                             name=style(
-                                f"$ echo '{json.dumps({'message':'hello!'})}' | "
+                                f"$ echo '{json.dumps({'message': 'hello!'})}' | "
                                 f"{ctx.command_path} HelloWorldFunction --event-file -"
                             ),
                             extra_row_modifiers=[ShowcaseRowModifier()],
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/invoke/core/command.py#L111'>samcli/commands/remote/invoke/core/command.py~L111</a>
```diff
                         RowDefinition(
                             name=style(
                                 f"${ctx.command_path} --stack-name mock-stack StockTradingStateMachine"
-                                f" -e '{json.dumps({'message':'hello!'})}'"
+                                f" -e '{json.dumps({'message': 'hello!'})}'"
                             ),
                             extra_row_modifiers=[ShowcaseRowModifier()],
                         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/invoke/core/command.py#L149'>samcli/commands/remote/invoke/core/command.py~L149</a>
```diff
                     formatter.write_rd([
                         RowDefinition(
                             name=style(
-                                f"$ echo '{json.dumps({'message':'hello!'})}' | "
+                                f"$ echo '{json.dumps({'message': 'hello!'})}' | "
                                 f"${ctx.command_path} --stack-name mock-stack StockTradingStateMachine"
                                 f" --parameter traceHeader=<>"
                             ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/invoke/core/command.py#L231'>samcli/commands/remote/invoke/core/command.py~L231</a>
```diff
                         RowDefinition(
                             name=style(
                                 f"${ctx.command_path} --stack-name mock-stack MyKinesisStream -e"
-                                f" '{json.dumps({'message':'hello!'})}'"
+                                f" '{json.dumps({'message': 'hello!'})}'"
                             ),
                             extra_row_modifiers=[ShowcaseRowModifier()],
                         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/samcli/commands/remote/test_event/put/core/command.py#L60'>samcli/commands/remote/test_event/put/core/command.py~L60</a>
```diff
                     ),
                     RowDefinition(
                         name=style(
-                            f"$ echo '{json.dumps({'message':'hello!'})}' | "
+                            f"$ echo '{json.dumps({'message': 'hello!'})}' | "
                             f"{ctx.command_path} --stack-name hello-world HelloWorldFunction --name MyEvent "
                             f"--file -"
                         ),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/buildcmd/test_build_cmd.py#L2139'>tests/integration/buildcmd/test_build_cmd.py~L2139</a>
```diff
             "Function2Handler": "main.second_function_handler",
             "FunctionRuntime": "3.7",
             "DockerFile": "Dockerfile",
-            "Tag": f"{random.randint(1,100)}",
+            "Tag": f"{random.randint(1, 100)}",
         }
         cmdlist = self.get_command_list(parameter_overrides=overrides)
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/buildcmd/test_build_cmd.py#L2826'>tests/integration/buildcmd/test_build_cmd.py~L2826</a>
```diff
         overrides = {
             "Runtime": "3.7",
             "DockerFile": "Dockerfile",
-            "Tag": f"{random.randint(1,100)}",
+            "Tag": f"{random.randint(1, 100)}",
             "LocalNestedFuncHandler": "main.handler",
         }
         cmdlist = self.get_command_list(
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/local/start_lambda/test_start_lambda.py#L450'>tests/integration/local/start_lambda/test_start_lambda.py~L450</a>
```diff
 class TestImagePackageType(StartLambdaIntegBaseClass):
     template_path = "/testdata/start_api/image_package_type/template.yaml"
     build_before_invoke = True
-    tag = f"python-{random.randint(1000,2000)}"
+    tag = f"python-{random.randint(1000, 2000)}"
     build_overrides = {"Tag": tag}
     parameter_overrides = {"ImageUri": f"helloworldfunction:{tag}"}
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/local/start_lambda/test_start_lambda.py#L480'>tests/integration/local/start_lambda/test_start_lambda.py~L480</a>
```diff
     template_path = "/testdata/start_api/image_package_type/template.yaml"
     container_mode = ContainersInitializationMode.EAGER.value
     build_before_invoke = True
-    tag = f"python-{random.randint(1000,2000)}"
+    tag = f"python-{random.randint(1000, 2000)}"
     build_overrides = {"Tag": tag}
     parameter_overrides = {"ImageUri": f"helloworldfunction:{tag}"}
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/local/start_lambda/test_start_lambda.py#L510'>tests/integration/local/start_lambda/test_start_lambda.py~L510</a>
```diff
     template_path = "/testdata/start_api/image_package_type/template.yaml"
     container_mode = ContainersInitializationMode.LAZY.value
     build_before_invoke = True
-    tag = f"python-{random.randint(1000,2000)}"
+    tag = f"python-{random.randint(1000, 2000)}"
     build_overrides = {"Tag": tag}
     parameter_overrides = {"ImageUri": f"helloworldfunction:{tag}"}
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/code/after/function/app.py#L10'>tests/integration/testdata/sync/code/after/function/app.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+2}",
+            "message": f"{layer_method() + 2}",
             "message_from_layer": f"{layer_method()}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/code/before/function/app.py#L10'>tests/integration/testdata/sync/code/before/function/app.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+1}",
+            "message": f"{layer_method() + 1}",
             "message_from_layer": f"{layer_method()}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/after/Python/function/app.py#L10'>tests/integration/testdata/sync/infra/after/Python/function/app.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+2}",
+            "message": f"{layer_method() + 2}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/before/Python/function/app.py#L10'>tests/integration/testdata/sync/infra/before/Python/function/app.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+1}",
+            "message": f"{layer_method() + 1}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/cdk/after/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py#L10'>tests/integration/testdata/sync/infra/cdk/after/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+2}",
+            "message": f"{layer_method() + 2}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/cdk/after/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py#L10'>tests/integration/testdata/sync/infra/cdk/after/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+2}",
+            "message": f"{layer_method() + 2}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/cdk/before/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py#L10'>tests/integration/testdata/sync/infra/cdk/before/asset.6598609927b272b36fdf01072092f9851ddcd1b41ba294f736ce77091f5cc456/main.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+1}",
+            "message": f"{layer_method() + 1}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/infra/cdk/before/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py#L10'>tests/integration/testdata/sync/infra/cdk/before/asset.b998895901bf33127f2c9dce715854f8b35aa73fb7eb5245ba9721580bbe5837/main.py~L10</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+1}",
+            "message": f"{layer_method() + 1}",
             "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist(),  # checking external library call will succeed
         }),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/nested/after/child_stack/child_functions/child_function.py#L7'>tests/integration/testdata/sync/nested/after/child_stack/child_functions/child_function.py~L7</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}"}),
+        "body": json.dumps({"message": f"{layer_method() + 6}"}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/nested/after/root_function/root_function.py#L17'>tests/integration/testdata/sync/nested/after/root_function/root_function.py~L17</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}", "location": ip.text.replace("\n", "")}),
+        "body": json.dumps({"message": f"{layer_method() + 6}", "location": ip.text.replace("\n", "")}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/nested/before/child_stack/child_functions/child_function.py#L7'>tests/integration/testdata/sync/nested/before/child_stack/child_functions/child_function.py~L7</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+5}"}),
+        "body": json.dumps({"message": f"{layer_method() + 5}"}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/nested/before/root_function/root_function.py#L17'>tests/integration/testdata/sync/nested/before/root_function/root_function.py~L17</a>
```diff
 
     return {
         "statusCode": 200,
-        "body": json.dumps({"message": f"{layer_method()+6}", "location": ip.text.replace("\n", "")}),
+        "body": json.dumps({"message": f"{layer_method() + 6}", "location": ip.text.replace("\n", "")}),
     }
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/integration/testdata/sync/nested_intrinsics/before/child_stack/child_function/function/function.py#L19'>tests/integration/testdata/sync/nested_intrinsics/before/child_stack/child_function/function/function.py~L19</a>
```diff
     return {
         "statusCode": 200,
         "body": json.dumps({
-            "message": f"{layer_method()+1}",
+            "message": f"{layer_method() + 1}",
             "location": ip.text.replace("\n", ""),
             # "extra_message": np.array([1, 2, 3, 4, 5, 6]).tolist() # checking external library call will succeed
         }),
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/unit/lib/build_module/test_build_graph.py#L311'>tests/unit/lib/build_module/test_build_graph.py~L311</a>
```diff
     handler = "{HANDLER}"
     functions = ["HelloWorldPython", "HelloWorld2Python"]
     [function_build_definitions.{UUID}.metadata]
-    Test = "{METADATA['Test']}"
-    Test2 = "{METADATA['Test2']}"
+    Test = "{METADATA["Test"]}"
+    Test2 = "{METADATA["Test2"]}"
     [function_build_definitions.{UUID}.env_vars]
-    env_vars = "{ENV_VARS['env_vars']}"
+    env_vars = "{ENV_VARS["env_vars"]}"
 
     [layer_build_definitions]
     [layer_build_definitions.{LAYER_UUID}]
```
<a href='https://github.com/aws/aws-sam-cli/blob/2d748b9d48dfc8ca204b4a100891681965059915/tests/unit/lib/build_module/test_build_graph.py#L327'>tests/unit/lib/build_module/test_build_graph.py~L327</a>
```diff
     manifest_hash = "{MANIFEST_HASH}"
     layer = "SumLayer"
     [layer_build_definitions.{LAYER_UUID}.env_vars]
-    env_vars = "{ENV_VARS['env_vars']}"
+    env_vars = "{ENV_VARS["env_vars"]}"
     """
 
     def test_should_instantiate_first_time(self):
```

</p>
</details>
<details><summary><a href="https://github.com/bloomberg/pytest-memray">bloomberg/pytest-memray</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bloomberg/pytest-memray/blob/1549d621b34d7cd726b8b4ee4dc212e1c9a0af3a/src/pytest_memray/marks.py#L159'>src/pytest_memray/marks.py~L159</a>
```diff
         stacks_left = num_stacks
         for function, file, line in stack_trace:
             if stacks_left <= 0:
-                text_lines.append(f"{padding*2}...")
+                text_lines.append(f"{padding * 2}...")
                 break
-            text_lines.append(f"{padding*2}{function}:{file}:{line}")
+            text_lines.append(f"{padding * 2}{function}:{file}:{line}")
             stacks_left -= 1
 
     return "\n".join(text_lines)
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+27 -27 lines across 14 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend_two_dimensions.py#L16'>examples/basic/annotations/legend_two_dimensions.py~L16</a>
```diff
 sinx = np.sin(x)
 
 p1 = figure(title="Default legend layout", width=500, height=300)
-[p1.line(x, (1 + i / 20) * sinx, legend_label=f"{1+i/20:.2f}*sin(x)") for i in range(7)]
+[p1.line(x, (1 + i / 20) * sinx, legend_label=f"{1 + i / 20:.2f}*sin(x)") for i in range(7)]
 
 p2 = figure(title="Legend layout with 2 columns", width=500, height=300)
-[p2.line(x, (1 + i / 20) * sinx, legend_label=f"{1+i/20:.2f}*sin(x)") for i in range(7)]
+[p2.line(x, (1 + i / 20) * sinx, legend_label=f"{1 + i / 20:.2f}*sin(x)") for i in range(7)]
 p2.legend.ncols = 2
 
 p3 = figure(title="Legend layout with 3 rows", width=500, height=300)
-[p3.line(x, (1 + i / 20) * sinx, legend_label=f"{1+i/20:.2f}*sin(x)") for i in range(7)]
+[p3.line(x, (1 + i / 20) * sinx, legend_label=f"{1 + i / 20:.2f}*sin(x)") for i in range(7)]
 p3.legend.nrows = 3
 
 show(column(p1, p2, p3))
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/fourier_animated.py#L62'>examples/server/app/fourier_animated.py~L62</a>
```diff
 terms_plot.circle(0, 0, radius=A, color=palette, line_width=2, line_dash=dashing, fill_color=None)
 
 for i in range(4):
-    legend_label = f"4sin({i*2+1}x)/{i*2+1}pi" if i else "4sin(x)/pi"
+    legend_label = f"4sin({i * 2 + 1}x)/{i * 2 + 1}pi" if i else "4sin(x)/pi"
     terms_plot.line("x", f"y{i}", color=palette[i], line_width=2, source=lines_source, legend_label=legend_label)
 
 terms_plot.circle("xterm-dot", "yterm-dot", size=5, color="color", source=items_source)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L51'>examples/styling/mathtext/latex_schrodinger.py~L51</a>
```diff
     p.line(q, y, color="red", line_width=2)
 
     p.add_layout(Label(x=-5.8, y=E_v, y_offset=-21, text=rf"$$v = {v}$$"))
-    p.add_layout(Label(x=3.9, y=E_v, y_offset=-25, text=rf"$$E_{v} = ({2*v+1}/2) \hbar\omega$$"))
+    p.add_layout(Label(x=3.9, y=E_v, y_offset=-25, text=rf"$$E_{v} = ({2 * v + 1}/2) \hbar\omega$$"))
 
 V = q**2 / 2
 p.line(q, V, line_color="black", line_width=2, line_dash="dashed")
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/hierarchical/crosstab.py#L37'>examples/topics/hierarchical/crosstab.py~L37</a>
```diff
     p.hbar(y=value(y), left=left, right=right, source=source, height=0.9, color=factor_cmap("Region", "MediumContrast4", regions))
 
     pcts = source.data[y]
-    source.data[f"{y} text"] = [f"{r}\n{x*100:0.1f}%" for r, x in zip(regions, pcts)]
+    source.data[f"{y} text"] = [f"{r}\n{x * 100:0.1f}%" for r, x in zip(regions, pcts)]
 
     p.text(y=value(y), x=left, text=f"{y} text", source=source, x_offset=10, text_color="color", text_baseline="middle", text_font_size="15px")
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/hierarchical/crosstab.py#L45'>examples/topics/hierarchical/crosstab.py~L45</a>
```diff
 
 p.hbar(right=0, left=-totals, y=totals.index, height=0.9, color="#dadada")
 
-text = [f"{name} ({totals.loc[name]*100:0.1f}%)" for name in cats]
+text = [f"{name} ({totals.loc[name] * 100:0.1f}%)" for name in cats]
 p.text(y=cats, x=0, text=text, text_baseline="middle", text_align="right", x_offset=-12, text_color="#4a4a4a", text_font_size="20px", text_font_style="bold")
 
 show(p)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/pyramid.py#L34'>examples/topics/stats/pyramid.py~L34</a>
```diff
 for i, (count, age) in enumerate(zip(f_hist, edges[1:])):
     if i % 2 == 1:
         continue
-    p.text(x=count, y=edges[1:][i], text=[f"{age-bin_width}-{age}yrs"], x_offset=5, y_offset=7, text_font_size="12px", text_color=DarkText[5])
+    p.text(x=count, y=edges[1:][i], text=[f"{age - bin_width}-{age}yrs"], x_offset=5, y_offset=7, text_font_size="12px", text_color=DarkText[5])
 
 # customise x-axis and y-axis
 p.xaxis.ticker = (-80, -60, -40, -20, 0, 20, 40, 60, 80)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L83'>setup.py~L83</a>
```diff
             stamp, txt = m.groups()
             msg.append(f"   {dim(green(stamp))} {dim(txt)}")
     print(BUILD_SUCCESS_MSG.format(msg="\n".join(msg)))
-    print(f"\n Build time: {bright(yellow(f'{t1-t0:0.1f} seconds'))}\n")
+    print(f"\n Build time: {bright(yellow(f'{t1 - t0:0.1f} seconds'))}\n")
 
     print("Build artifact sizes:")
     try:
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L334'>src/bokeh/colors/color.py~L334</a>
```diff
 
         """
         if self.a < 1.0:
-            return f"#{self.r:02x}{self.g:02x}{self.b:02x}{int(round(self.a*255)):02x}"
+            return f"#{self.r:02x}{self.g:02x}{self.b:02x}{int(round(self.a * 255)):02x}"
         else:
             return f"#{self.r:02x}{self.g:02x}{self.b:02x}"
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L458'>src/bokeh/colors/color.py~L458</a>
```diff
 
         """
         if self.a == 1.0:
-            return f"hsl({self.h}, {self.s*100}%, {self.l*100}%)"
+            return f"hsl({self.h}, {self.s * 100}%, {self.l * 100}%)"
         else:
-            return f"hsla({self.h}, {self.s*100}%, {self.l*100}%, {self.a})"
+            return f"hsla({self.h}, {self.s * 100}%, {self.l * 100}%, {self.a})"
 
     def to_hsl(self) -> HSL:
         """Return a HSL copy for this HSL color.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/either.py#L98'>src/bokeh/core/property/either.py~L98</a>
```diff
         if any(param.is_valid(value) for param in self.type_params):
             return
 
-        msg = "" if not detail else f"expected an element of either {nice_join([ str(param) for param in self.type_params ])}, got {value!r}"
+        msg = "" if not detail else f"expected an element of either {nice_join([str(param) for param in self.type_params])}, got {value!r}"
         raise ValueError(msg)
 
     def wrap(self, value):
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property_mixins.py#L233'>src/bokeh/core/property_mixins.py~L233</a>
```diff
 _hatch_pattern_help = f"""
 Built-in patterns are can either be specified as long names:
 
-{', '. join(HatchPattern)}
+{", ".join(HatchPattern)}
 
 or as one-letter abbreviations:
 
-{', '. join(repr(x) for x in HatchPatternAbbreviation)}
+{", ".join(repr(x) for x in HatchPatternAbbreviation)}
 """
 
 _hatch_weight_help = """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_model.py#L119'>src/bokeh/sphinxext/bokeh_model.py~L119</a>
```diff
         name_prefix, model_name, arglist, retann = m.groups()
 
         if getenv("BOKEH_SPHINX_QUICK") == "1":
-            return self.parse(f"{model_name}\n{'-'*len(model_name)}\n", "<bokeh-model>")
+            return self.parse(f"{model_name}\n{'-' * len(model_name)}\n", "<bokeh-model>")
 
         module_name = self.options["module"]
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/sampledata.py#L212'>src/bokeh/util/sampledata.py~L212</a>
```diff
             file.write(data)
 
             if progress:
-                status = f"\r{fetch_size:< 10d} [{fetch_size*100.0/file_size:6.2f}%%]"
+                status = f"\r{fetch_size:< 10d} [{fetch_size * 100.0 / file_size:6.2f}%%]"
                 stdout.write(status)
                 stdout.flush()
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L143'>tests/integration/widgets/test_select.py~L143</a>
```diff
             opts = grp.find_elements(By.TAG_NAME, "option")
             assert len(opts) == i
             for j, opt in enumerate(opts, 1):
-                assert opt.text == f"Option {i*10 + j}"
-                assert opt.get_attribute("value") == f"Option {i*10 + j}"
+                assert opt.text == f"Option {i * 10 + j}"
+                assert opt.get_attribute("value") == f"Option {i * 10 + j}"
 
         assert page.has_no_console_errors()
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L167'>tests/integration/widgets/test_select.py~L167</a>
```diff
             opts = grp.find_elements(By.TAG_NAME, "option")
             assert len(opts) == i
             for j, opt in enumerate(opts, 1):
-                assert opt.text == f"Option {i*10 + j}"
-                assert opt.get_attribute("value") == f"Option {i*10 + j}"
+                assert opt.text == f"Option {i * 10 + j}"
+                assert opt.get_attribute("value") == f"Option {i * 10 + j}"
 
         assert page.has_no_console_errors()
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L189'>tests/integration/widgets/test_select.py~L189</a>
```diff
             opts = grp.find_elements(By.TAG_NAME, "option")
             assert len(opts) == i
             for j, opt in enumerate(opts, 1):
-                assert opt.text == f"Option {i*10 + j}"
-                assert opt.get_attribute("value") == f"{i*10 + j}"
+                assert opt.text == f"Option {i * 10 + j}"
+                assert opt.get_attribute("value") == f"{i * 10 + j}"
 
         assert page.has_no_console_errors()
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L213'>tests/integration/widgets/test_select.py~L213</a>
```diff
             opts = grp.find_elements(By.TAG_NAME, "option")
             assert len(opts) == i
             for j, opt in enumerate(opts, 1):
-                assert opt.text == f"Option {i*10 + j}"
-                assert opt.get_attribute("value") == f"{i*10 + j}"
+                assert opt.text == f"Option {i * 10 + j}"
+                assert opt.get_attribute("value") == f"{i * 10 + j}"
 
         assert page.has_no_console_errors()
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_examples.py#L230'>tests/test_examples.py~L230</a>
```diff
     result = run_in_chrome(url)
     end = time.time()
 
-    info(f"Example rendered in {(end-start):.3f} seconds")
+    info(f"Example rendered in {(end - start):.3f} seconds")
 
     success = result["success"]
     timeout = result["timeout"]
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_bases.py#L113'>tests/unit/bokeh/core/property/test_bases.py~L113</a>
```diff
         def raise_(ex):
             raise ex
 
-        p.asserts(False, lambda obj, name, value: raise_(ValueError(f"bad {hp==obj} {name} {value}")))
+        p.asserts(False, lambda obj, name, value: raise_(ValueError(f"bad {hp == obj} {name} {value}")))
 
         with pytest.raises(ValueError) as e:
             p.prepare_value(hp, "foo", 10)
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+9 -9 lines across 8 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/car/vin.py#L67'>selfdrive/car/vin.py~L67</a>
```diff
                 except Exception:
                     cloudlog.exception("VIN query exception")
 
-        cloudlog.error(f"vin query retry ({i+1}) ...")
+        cloudlog.error(f"vin query retry ({i + 1}) ...")
 
     return -1, -1, VIN_UNKNOWN
 
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/controls/lib/events.py#L382'>selfdrive/controls/lib/events.py~L382</a>
```diff
 def joystick_alert(CP: car.CarParams, CS: car.CarState, sm: messaging.SubMaster, metric: bool, soft_disable_time: int) -> Alert:
     axes = sm["testJoystick"].axes
     gb, steer = list(axes)[:2] if len(axes) else (0.0, 0.0)
-    vals = f"Gas: {round(gb * 100.)}%, Steer: {round(steer * 100.)}%"
+    vals = f"Gas: {round(gb * 100.0)}%, Steer: {round(steer * 100.0)}%"
     return NormalPermanentAlert("Joystick Mode", vals)
 
 
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/debug/count_events.py#L65'>selfdrive/debug/count_events.py~L65</a>
```diff
     for k, v in cnt_cameras.items():
         s = SERVICE_LIST[k]
         expected_frames = int(s.frequency * duration / cast(float, s.decimation))
-        print("  ", k.ljust(20), f"{v}, {v/expected_frames:.1%} of expected")
+        print("  ", k.ljust(20), f"{v}, {v / expected_frames:.1%} of expected")
 
     print("\n")
     print("Alerts")
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/modeld/tests/timing/benchmark.py#L35'>selfdrive/modeld/tests/timing/benchmark.py~L35</a>
```diff
     print("\n\n")
     print(f"ran modeld {N} times for {TIME}s each")
     for _, t in enumerate(execution_times):
-        print(f"\tavg: {sum(t)/len(t):0.2f}ms, min: {min(t):0.2f}ms, max: {max(t):0.2f}ms")
+        print(f"\tavg: {sum(t) / len(t):0.2f}ms, min: {min(t):0.2f}ms, max: {max(t):0.2f}ms")
     print("\n\n")
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/navd/navd.py#L144'>selfdrive/navd/navd.py~L144</a>
```diff
             waypoint_coords = json.loads(waypoints)
 
         coords = [(self.last_position.longitude, self.last_position.latitude), *waypoint_coords, (destination.longitude, destination.latitude)]
-        params["waypoints"] = f"0;{len(coords)-1}"
+        params["waypoints"] = f"0;{len(coords) - 1}"
         if self.last_bearing is not None:
             params["bearings"] = f"{(self.last_bearing + 360) % 360:.0f},90" + (";" * (len(coords) - 1))
 
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/selfdrive/test/test_onroad.py#L394'>selfdrive/test/test_onroad.py~L394</a>
```diff
                 result += f"{s} - failed RSD timing check\n"
                 passed = False
 
-            result += f"{s.ljust(40)}: {np.array([np.mean(ts), np.max(ts), np.min(ts)])*1e3}\n"
-            result += f"{''.ljust(40)}  {np.max(np.absolute([np.max(ts)/dt, np.min(ts)/dt]))} {np.std(ts)/dt}\n"
+            result += f"{s.ljust(40)}: {np.array([np.mean(ts), np.max(ts), np.min(ts)]) * 1e3}\n"
+            result += f"{''.ljust(40)}  {np.max(np.absolute([np.max(ts) / dt, np.min(ts) / dt]))} {np.std(ts) / dt}\n"
         result += "=" * 67
         print(result)
         self.assertTrue(passed)
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/system/loggerd/tests/test_encoder.py#L141'>system/loggerd/tests/test_encoder.py~L141</a>
```diff
             for i in trange(num_segments):
                 # poll for next segment
                 with Timeout(int(SEGMENT_LENGTH * 10), error_msg=f"timed out waiting for segment {i}"):
-                    while Path(f"{route_prefix_path}--{i+1}") not in Path(Paths.log_root()).iterdir():
+                    while Path(f"{route_prefix_path}--{i + 1}") not in Path(Paths.log_root()).iterdir():
                         time.sleep(0.1)
                 check_seg(i)
         finally:
```
<a href='https://github.com/commaai/openpilot/blob/5a441ec0c4bf39a5e066e307ee6c9a8b55f8d8b5/tools/tuning/measure_steering_accuracy.py#L110'>tools/tuning/measure_steering_accuracy.py~L110</a>
```diff
             for group in self.display_groups:
                 if len(self.speed_group_stats[group]) > 0:
                     print(f"speed group: {group:10s} {self.all_groups[group][1]:>96s}")
-                    print(f"  {'-'*118}")
+                    print(f"  {'-' * 118}")
                     for k in sorted(self.speed_group_stats[group].keys()):
                         v = self.speed_group_stats[group][k]
                         print(
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+218 -193 lines across 62 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L69'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py~L69</a>
```diff
         except ConnectionError as exception:  # pragma: no cover
             # Get originating Exception in Exception chain
             error_class = str(exception.__class__)  # pragma: no cover
-            err_type = f"""<{error_class[error_class.find("'") + 1: error_class.rfind("'")]}>"""  # pragma: no cover
+            err_type = f"""<{error_class[error_class.find("'") + 1 : error_class.rfind("'")]}>"""  # pragma: no cover
             err_msg = (
                 "Verify that the server URL parameter"
                 " is correct and that you have access to the server from your host."
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/Anomali_ThreatStream/Integrations/AnomaliThreatStreamv3/AnomaliThreatStreamv3.py#L1782'>Packs/Anomali_ThreatStream/Integrations/AnomaliThreatStreamv3/AnomaliThreatStreamv3.py~L1782</a>
```diff
     readable_output: str = ""
     if associated_entity_ids_results == num_associated_entity_ids:
         readable_output = (
-            f'The {associated_entity_type} entities with ids {", ".join(map(str, res.get("ids",[])))} '
+            f'The {associated_entity_type} entities with ids {", ".join(map(str, res.get("ids", [])))} '
             f'were associated successfully to entity id: {entity_id}.'
         )
     elif associated_entity_ids_results > 0:
         readable_output = (
-            f'Part of the {associated_entity_type} entities with ids {", ".join(map(str, res.get("ids",[])))} '
+            f'Part of the {associated_entity_type} entities with ids {", ".join(map(str, res.get("ids", [])))} '
             f'were associated successfully to entity id: {entity_id}.'
         )
     else:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/Anomali_ThreatStream/Scripts/ThreatstreamBuildIocImportJson/ThreatstreamBuildIocImportJson.py#L60'>Packs/Anomali_ThreatStream/Scripts/ThreatstreamBuildIocImportJson/ThreatstreamBuildIocImportJson.py~L60</a>
```diff
         if not re.match(domainRegex, domain):
             invalid_indicators.append(domain)
     if len(invalid_indicators) > 0:
-        raise DemistoException(f'Invalid indicators values: {", ".join(map(str,invalid_indicators))}')
+        raise DemistoException(f'Invalid indicators values: {", ".join(map(str, invalid_indicators))}')
 
 
 def get_indicators_from_user(args: dict, indicators_types: dict) -> list:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule.py#L192'>Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule.py~L192</a>
```diff
             start = 1
             for lines in batch(f.readlines(), 100):
                 end = start + len(lines)
-                demisto.info(f"nginx access log ({start}-{end-1}): " + "".join(lines))
+                demisto.info(f"nginx access log ({start}-{end - 1}): " + "".join(lines))
                 start = end
         Path(old_access).unlink()
     if log_error:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule.py#L200'>Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule.py~L200</a>
```diff
             start = 1
             for lines in batch(f.readlines(), 100):
                 end = start + len(lines)
-                demisto.error(f"nginx error log ({start}-{end-1}): " + "".join(lines))
+                demisto.error(f"nginx error log ({start}-{end - 1}): " + "".join(lines))
                 start = end
         Path(old_error).unlink()
 
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/ArcusTeam/Integrations/ArcusTeam/ArcusTeam.py#L71'>Packs/ArcusTeam/Integrations/ArcusTeam/ArcusTeam.py~L71</a>
```diff
     markdown += f"**Series**: {device.get('series')}{nl}"
     markdown += f"**Categories**: {','.join(device.get('categories'))}{nl}"
     markdown += f"**DeviceID**: {device.get('device_key')}{nl}"
-    markdown += f"**Match Score**: {round(device.get('score')*100,2)}%{nl}"
+    markdown += f"**Match Score**: {round(device.get('score') * 100, 2)}%{nl}"
     firmwares = device.get("firmware")
     markdown += tableToMarkdown("Firmwares", firmwares, headers=["firmwareid", "version", "name"])
     return markdown
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L3038'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py~L3038</a>
```diff
     """
 
     stmt = oper_between_filters.join(
-        f"'{filter_key}' {oper_in_filter} \"{wrap_filter_value(filter_val,oper_in_filter)}\""
+        f"'{filter_key}' {oper_in_filter} \"{wrap_filter_value(filter_val, oper_in_filter)}\""
         for filter_key, filter_val in (filter_mapper).items()
     )
     return stmt
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/BreachRx/Integrations/BreachRx/BreachRx.py#L158'>Packs/BreachRx/Integrations/BreachRx/BreachRx.py~L158</a>
```diff
         description = f"""An Incident copied from the Palo Alto Networks XSOAR platform.
             <br>
             <br>
-            XSOAR Incident Name: {demisto.incident().get('name')}"""
+            XSOAR Incident Name: {demisto.incident().get("name")}"""
 
     response = client.create_incident(incident_name, description)
 
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/Campaign/Scripts/GetCampaignIndicatorsByIncidentId/GetCampaignIndicatorsByIncidentId.py#L26'>Packs/Campaign/Scripts/GetCampaignIndicatorsByIncidentId/GetCampaignIndicatorsByIncidentId.py~L26</a>
```diff
     Returns:
         List of the campaign indicators.
     """
-    indicators_query = f"""investigationIDs:({' '.join(f'"{id_}"' for id_ in incident_ids)})"""
+    indicators_query = f"""investigationIDs:({" ".join(f'"{id_}"' for id_ in incident_ids)})"""
     fields = ["id", "indicator_type", "investigationIDs", "investigationsCount", "score", "value"]
     search_indicators = IndicatorsSearcher(query=indicators_query, limit=150, size=500, filter_fields=",".join(fields))
     indicators: list[dict] = []
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1170'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1170</a>
```diff
                             self.pal = True
                             t_bmp = self.PALETTE + self.bdat[len(t_hdr) : len(t_hdr) + cf * t_width * t_height]
                         else:
-                            self.b_log("error", False, f"Unexpected bpp {8*cf} found during processing; aborting.")
+                            self.b_log("error", False, f"Unexpected bpp {8 * cf} found during processing; aborting.")
                     bl = cf * 64 * 64
             if len(t_bmp) > 0:
                 self.bmps.append(t_bmp)
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommonScripts/Scripts/GetIndicatorDBotScoreFromCache/GetIndicatorDBotScoreFromCache.py#L7'>Packs/CommonScripts/Scripts/GetIndicatorDBotScoreFromCache/GetIndicatorDBotScoreFromCache.py~L7</a>
```diff
     values: list[str] = argToList(demisto.args().get("value", None))
     unique_values: set[str] = {v.lower() for v in values}  # search query is case insensitive
 
-    query = f"""value:({' '.join([f'"{value}"' for value in unique_values])})"""
+    query = f"""value:({" ".join([f'"{value}"' for value in unique_values])})"""
     demisto.debug(f"{query=}")
 
     res = demisto.searchIndicators(
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2.py#L30'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2.py~L30</a>
```diff
     if parent_email_file:
         md += f"### Containing email: {parent_email_file}\n"
 
-    md += f"""* From:\t{email_data.get('From') or ""}\n"""
-    md += f"""* To:\t{email_data.get('To') or ""}\n"""
-    md += f"""* CC:\t{email_data.get('CC') or ""}\n"""
-    md += f"""* BCC:\t{email_data.get('BCC') or ""}\n"""
-    md += f"""* Subject:\t{email_data.get('Subject') or ""}\n"""
+    md += f"""* From:\t{email_data.get("From") or ""}\n"""
+    md += f"""* To:\t{email_data.get("To") or ""}\n"""
+    md += f"""* CC:\t{email_data.get("CC") or ""}\n"""
+    md += f"""* BCC:\t{email_data.get("BCC") or ""}\n"""
+    md += f"""* Subject:\t{email_data.get("Subject") or ""}\n"""
     if email_data.get("Text"):
         text = email_data["Text"].replace("<", "[").replace(">", "]")
         md += f'* Body/Text:\t{text or ""}\n'
     if email_data.get("HTML"):
-        md += f"""* Body/HTML:\t{email_data['HTML'] or ""}\n"""
+        md += f"""* Body/HTML:\t{email_data["HTML"] or ""}\n"""
 
-    md += f"""* Attachments:\t{email_data.get('Attachments') or ""}\n"""
+    md += f"""* Attachments:\t{email_data.get("Attachments") or ""}\n"""
     md += "\n\n" + tableToMarkdown("HeadersMap", email_data.get("HeadersMap"))
     return md
 
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommunityCommonScripts/Scripts/RetrievePlaybooksAndIntegrations/RetrievePlaybooksAndIntegrations.py#L66'>Packs/CommunityCommonScripts/Scripts/RetrievePlaybooksAndIntegrations/RetrievePlaybooksAndIntegrations.py~L66</a>
```diff
 def retrieve_playbooks_and_integrations(args: Dict[str, Any]) -> CommandResults:
     playbooks: List[str] = []
     integrations: List[str] = []
-    query = f'''name:"{args['playbook_name']}"'''
+    query = f'''name:"{args["playbook_name"]}"'''
     body = {"query": query}
     playbooks_json = perform_rest_call("post", "playbook/search", body)
     for playbook_json in playbooks_json["playbooks"]:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommunityCommonScripts/Scripts/RetrievePlaybooksAndIntegrations/RetrievePlaybooksAndIntegrations.py#L84'>Packs/CommunityCommonScripts/Scripts/RetrievePlaybooksAndIntegrations/RetrievePlaybooksAndIntegrations.py~L84</a>
```diff
     outputs = {"Playbooks": playbooks, "Integrations": integrations}
 
     return CommandResults(
-        readable_output=f'''Retrieved Playbooks and Integrations for Playbook "{playbook_json['name']}"''',
+        readable_output=f'''Retrieved Playbooks and Integrations for Playbook "{playbook_json["name"]}"''',
         outputs_prefix="RetrievePlaybooksAndIntegrations",
         outputs_key_field="",
         outputs=outputs,
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CommvaultSecurityIQ/Integrations/CommvaultSecurityIQ/CommvaultSecurityIQ.py#L881'>Packs/CommvaultSecurityIQ/Integrations/CommvaultSecurityIQ/CommvaultSecurityIQ.py~L881</a>
```diff
             self.validate_session_or_generate_token(self.current_api_token)
             response = self.http_request("GET", f"/V4/SAML/{identity_server_name}")
             if "error" in response:
-                demisto.debug(f"Error [{response.get('error',{}).get('errorString','')}]")
+                demisto.debug(f"Error [{response.get('error', {}).get('errorString', '')}]")
                 return False
             if response.get("enabled"):
                 demisto.debug(f"SAML is enabled for identity server [{identity_server_name}]. Going to disable it")
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/ContentTesting/Scripts/UnitTestCoverage/UnitTestCoverage.py#L34'>Packs/ContentTesting/Scripts/UnitTestCoverage/UnitTestCoverage.py~L34</a>
```diff
         markdown += "|No Tasks Found||||\n"
         return markdown
     for _key, val in tasks.items():
-        markdown += f"|{val['name']}|{val['count']}|{val['completed']}|{val['completed']/val['count']*100}%|\n"
+        markdown += f"|{val['name']}|{val['count']}|{val['completed']}|{val['completed'] / val['count'] * 100}%|\n"
 
     return markdown
 
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CovalenceForSecurityProviders/Integrations/CovalenceForSecurityProviders/CovalenceForSecurityProviders.py#L168'>Packs/CovalenceForSecurityProviders/Integrations/CovalenceForSecurityProviders/CovalenceForSecurityProviders.py~L168</a>
```diff
                 created_time_str = created_time.strftime(DATE_FORMAT)
 
                 if BROKER:
-                    incident_name = f"""[{target_org}] [{a.get('type', 'No alert type')}] {a.get('analystTitle', 'No title')}"""
+                    incident_name = f"""[{target_org}] [{a.get("type", "No alert type")}] {a.get("analystTitle", "No title")}"""
                 else:
-                    incident_name = f"""[{a.get('type', 'No alert type')}] {a.get('analystTitle', 'No title')}"""
+                    incident_name = f"""[{a.get("type", "No alert type")}] {a.get("analystTitle", "No title")}"""
                 incident: Dict[str, Any] = {"name": incident_name, "occured": created_time_str, "rawJSON": json.dumps(a)}
                 if a.get("severity", None):
                     #  XSOAR mapping
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CovalenceManagedSecurity/Integrations/CovalenceManagedSecurity/CovalenceManagedSecurity.py#L236'>Packs/CovalenceManagedSecurity/Integrations/CovalenceManagedSecurity/CovalenceManagedSecurity.py~L236</a>
```diff
                 if a.get("steps", None) and len(a["steps"]) > 0:
                     incident["details"] += "\n\nMitigation Steps\n"
                     for step in a["steps"]:
-                        incident["details"] += f"""- {step['label']}\n"""
+                        incident["details"] += f"""- {step["label"]}\n"""
                 if org_id:
                     active_response_profile = p.get_active_response_profile(org_id)
                     if active_response_profile:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/CrowdStrikeFalcon/Integrations/CrowdStrikeFalcon/CrowdStrikeFalcon.py#L6073'>Packs/CrowdStrikeFalcon/Integrations/CrowdStrikeFalcon/CrowdStrikeFalcon.py~L6073</a>
```diff
 def ODS_create_scan_request(args: dict, is_scheduled: bool) -> dict:
     body = make_create_scan_request_body(args, is_scheduled)
     remove_nulls_from_dictionary(body)
-    return http_request("POST", f'/ods/entities/{"scheduled-"*is_scheduled}scans/v1', json=body)
+    return http_request("POST", f'/ods/entities/{"scheduled-" * is_scheduled}scans/v1', json=body)
 
 
 def ODS_verify_create_scan_command(args: dict) -> None:
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/Cryptosim/Integrations/Cryptosim/Cryptosim.py#L132'>Packs/Cryptosim/Integrations/Cryptosim/Cryptosim.py~L132</a>
```diff
             message = "ok"
         else:
             raise Exception(f"""StatusCode:
-                            {client.correlations().get('StatusCode')},
-                            Error: {client.correlations().get('ErrorMessage')}
+                            {client.correlations().get("StatusCode")},
+                            Error: {client.correlations().get("ErrorMessage")}
                             """)
     except DemistoException as e:
         if "401" in str(e):
```
<a href='https://github.com/demisto/content/blob/638687e520d706b20ca851ab4cfc90ad2554c20f/Packs/Cybereason/Integrations/Cybereason/Cybereason.py#L875'>Packs/Cybereason/Integrations/Cybereason/Cybereason.py~L875</a>
```diff
         response = get_remediation_action(client, malop_guid, machine_name, target_id, remediation_action)
         action_status = get_remediation_action_status(client, user_name, malop_guid, response, comment)
         if dict_safe_get(action_status, ["Remediation status"]) == "SUCCESS":
-            success_response = f"""Kill process remediation action status is: {dict_safe_get(
-                action_status, ['Remediation status'])} \n Remediation ID: {dict_safe_get(action_status, ['Remediation ID'])}"""
+            success_response = f"""Kill process remediation action status is: {
+                dict_safe_get(action_status, ["Remediation status"])
+            } \n Remediation ID: {dict_safe_get(action_status, ["Remediation ID"])}"""
             return CommandResults(readable_output=success_response)
         elif dict_sa...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2024-02-13 19:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-formatting)

### Merging #9642 will **not alter performance**

<sub>Comparing <code>dhruv/fstring-formatting</code> (7eed676) with <code>main</code> (fe79798)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:171 on 2024-02-13 20:06_

Nit: Can we avoid this as well if we have a simple attribute chain `a.b.c`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:188 on 2024-02-13 20:07_

I think we have to reuse the outer context here or you lose information (are we in a prenthesized context? what's the indent level and potentially other new state that will be added). 

The alternative is to clone the existing context and only change `in_f_string`.

You can reuse the context by building the `VecBuffer` manually

```python
let formatted = {
	let mut buffer = VecBuffer::new(f.state_mut());
	write!(buffer, [expression.format()])?;
	buffer.into_vec()
};

let print_options = f.context.options().as_print_options();
let expression_str = Printer::new(f.context().source_code(), print_options).print(&formatted)?;
```

---

_@MichaReiser reviewed on 2024-02-13 20:10_

---

_@MichaReiser reviewed on 2024-02-13 20:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/context.rs`:17 on 2024-02-13 20:17_

Nit: I would rename the enum to `FStringContext` or similar and have a variant `Inside` or `Outside` because the enum doesn't represent arbitrary expression contexts.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:86 on 2024-02-13 20:18_

Nit: I would prefer if we call this `fstring_formatting`. Not everyone knows all pep's by heart ;)

Does this gate the entire f string formatting or only the multiline fstring formatting? 

---

_@MichaReiser reviewed on 2024-02-13 20:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:69 on 2024-02-13 20:22_

I think we need to change this to only return `true` when the string literals are multiline or we risk instability if an f-string becomes single line because we collapse the expression part. Or is this handled somewhere else?

---

_@MichaReiser reviewed on 2024-02-13 20:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/context.rs`:21 on 2024-02-13 20:23_

What's the size of `PyFormatContext`. I wonder if the added field pushes it over the limit so that llvm/rust opts out of some optimisations. Or try re-running the benchmarks to see if the performance still regresses. 

What I find interesting is that the regressing benchmark contains exactly one f-string.

---

_@MichaReiser reviewed on 2024-02-13 20:23_

---

_@dhruvmanila reviewed on 2024-02-14 05:04_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/preview.rs`:86 on 2024-02-14 05:04_

> Does this gate the entire f string formatting or only the multiline fstring formatting?

The entire f-string formatting. If not enabled, it'll fallback to the previous behavior of normalizing the f-string.

---

_@dhruvmanila reviewed on 2024-02-14 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/context.rs`:17 on 2024-02-14 05:08_

`FStringContext` is already taken 😅

Let me think of something else. 

---

_@dhruvmanila reviewed on 2024-02-14 05:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:188 on 2024-02-14 05:43_

We need to have different options (max line width, no magic trailing comma) which is why I choose to create a new context but I see your point. I'm using the following:

```rs
                    let context = f.context().clone().in_f_string(self.context.quotes());
                    let formatted = crate::format!(context, [expression.format()])?;
                    let printer_options = f
                        .options()
                        .clone()
                        .with_line_width(NonZeroU16::MAX.into())
                        .with_magic_trailing_comma(MagicTrailingComma::Ignore)
                        .as_print_options();
                    text(
                        Printer::new(f.context().source_code(), printer_options)
                            .print(formatted.document())?
                            .as_code(),
                    )
                    .fmt(f)?;
```

---

_@dhruvmanila reviewed on 2024-02-14 12:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:188 on 2024-02-14 12:46_

This doesn't work because the options need to be updated for the current context as well.

---

_@dhruvmanila reviewed on 2024-02-14 13:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/context.rs`:21 on 2024-02-14 13:28_

The size is the same before and after (80).

---

_@dhruvmanila reviewed on 2024-02-14 13:53_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:171 on 2024-02-14 13:53_

I'm using the preorder visitor which should be more robust.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-02-14 20:02_

---

_Marked ready for review by @dhruvmanila on 2024-02-14 20:28_

---

_Comment by @MichaReiser on 2024-02-15 10:02_

I've some understand questions first:

> The heuristics for adding newline is similar to that of [Prettier](https://prettier.io/docs/en/next/rationale.html#template-literals) where the formatter would only split an expression in the replacement field across multiple lines if there was already a line break within the replacement field.

Is the logic if replacement expressions are allowed to expand over multiple lines globally for the entire f-string-literal (expand if any replacement expression contains a line break), or is the decision made for each replacement expression (only expand the replacement expressions that already contain line breaks, keep the other ones flat)?

> But, for triple-quoted strings, we can re-use the same quote char unless the inner string is itself a triple-quoted string.

Does this also apply to Python 3.12 or does Python 3.12 allow to reuse the same triple quotes?

> If debug expressions are present in the replacement field of a f-string, then the whitespace needs to be preserved as they will be rendered as it is (for example, f"{  x  = }". If there are any nested f-strings, then the whitespace in them needs to be preserved as well which means that we'll stop formatting the f-string as soon as we encounter a debug expression.

Does that mean we do not format any f-string that contains any debug expression or is it that we only disable formatting for replacements that contain debug expressions?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:72 on 2024-02-15 10:12_

Nit: It can be helpful to number comments (or assignments as well). It makes it easier to identify which comment is causing issues (if it's just comment, than it's one of the 20 or so comments in this file)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:299 on 2024-02-15 10:15_

When I run this example in the playground after changing the implementation to only use `handle_bracketed_end_of_line_comment` then the comment becomes a trailing comment of `.3f` which seems correct to me (more than making it a dangling comment)

```
{
    Node {
        kind: FStringLiteralElement,
        range: 21..24,
        source: `.3f`,
    }: {
        "leading": [],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# comment",
                position: OwnLine,
                formatted: false,
            },
        ],
    },
}
```

Why is it necessary to make the comment a danling comment?

Conceptually I would prefer if the comment remains a trailing comment. It being a dangling comment would take me by surprise.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/context.rs`:9 on 2024-02-15 10:18_

Nit: Can we move this further down in the file. All other "inner" types are placed after `PyFormatContext`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/string_literal.rs`:4 on 2024-02-15 10:20_

Nit: Revert the changes to this file

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:121 on 2024-02-15 10:22_

Is the reason that we don't need to gate this with the preview style that the existing f-string formatting never called into normalize for nested f-strings?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:42 on 2024-02-15 10:27_

We can avoid cloning the comments (it's not expensive but it still requires an atomic increment and decrement) by moving the use after the `fmt` call
```suggestion
        let string = StringPart::from_source(self.value.range(), &locator);

        let normalizer = StringNormalizer::from_context(f.context())
            .with_quoting(self.quoting)
            .with_preferred_quote_style(f.options().quote_style());

        // If f-string formatting is disabled (not in preview), then we will
        // fall back to the previous behavior of normalizing the f-string.
        if !is_f_string_formatting_enabled(f.context()) {
            let result = normalizer.normalize(&string, &locator).fmt(f);
            let comments = f.context().comments();

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:51 on 2024-02-15 10:28_

To me as a reader it isn't clear what this TODO is about.

* Is this something that needs to be addressed before releasing?
* What could be simplified and how? 

What I understand is that this isn't something that needs addressing and is more an observation. I would remove the `TODO` and expand on what could be simplified with Python 3.12 and ideally, with a reason why we aren't doing it today.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:57 on 2024-02-15 10:29_

Nit: Could you link to Prettier's documentation. It might come handy if someone wants to explain our heuristic and have some references.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:68 on 2024-02-15 10:31_

Nit: I've duplicated the `memchr` call to find newlines a bunch and it seems you're doing the same :laughing: Should we extract this into a helper function: `contains_newline(ranged: impl Ranged, source: &str) -> bool `?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:86 on 2024-02-15 10:32_

The `format_with` here isn't necessary because you aren't nesting it in another format element or passing it to `write`
```suggestion
        f.join()
            .entries(
                self.value
                    .elements
                    .iter()
                    .map(|element| FormatFStringElement::new(element, context)),
            )
            .finish()?;

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:122 on 2024-02-15 10:36_

Nit: Maybe rename now that we're no longer using the `RemoveSoftlineBreakBuffer` to a more semantical name. An approach that we used in various places in the formatter is to call this `layout` and use a custom enum for it that allows documenting the layout:


```rust
#[derive(Copy, Clone, Debug)]
pub(crate) FStringLayout {
    /// Don't break replacement expressions to keep the string flat.
    Flat,
    /// Allow breaking replacemnet expressions across multiple lines.
    Multiline
}

impl FStringLayout {
    fn from_fstring(fstring: &...) -> Self {
        ...
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:231 on 2024-02-15 10:38_

I'm surprised that we still need the `RemoveSoftLinesBuffer`. I would have expected that we either use the Printer approach or use `RemoveSoftLInesBuffer` but not both.Can you explain what it is used for?

I'm also confused how this even works: 

* `RemoveSoftLinesBuffer::new` takes a `&mut Buffer` as input -> borrowing f mutably
* You write to `f` (which takes a `&mut Buffer`) in the code below... 

Why isn't this a borrow checker error?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:235 on 2024-02-15 10:41_

I'm surprised by the use of `soft_block_indent` in combination with `RemoveSoftLinesBuffer`. The soft line buffer will remove the `soft_line_break` inserted by `soft_block_indent` and the printer won't add an indent except if it sees a line break (which it doesn't because we made sure to remove all of them). Can you explain why `soft_block_indent` is necessary?

That should also allow us to remove the `group` (good for perf)
```suggestion
                        write!(buffer, [item])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:99 on 2024-02-15 10:44_

Nit: We can avoid cloning `comments` by moving the declaration into the if/else branch where we test if there's a debug expression and avoid cloning it if there's no debug expression.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:104 on 2024-02-15 10:46_

The term suppressed has a very specific meaning in the formatter. It means that the user disabled formatting by using a suppression comment. 
That's why I recommend you using a different term. 

*If the element has a debug text, preserve the same formatting as in the source (`verbatim`)*

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:116 on 2024-02-15 10:48_

We should probably not use `suppressed_node`. The builder is intended to be used for statements that are suppressed and it makes some assumptions about it (it's also marked as `cold` which doesn't apply for you). 

I think what you want is to use `verbatim_text(verbatim_range)` which gives you the *format as in source* behavior.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:131 on 2024-02-15 10:50_

Same here: Use `verbatim_text` instead of `suppressed_node`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:144 on 2024-02-15 10:52_

Nit: Rename to `bracket_spacing`. It's not really a line break or space because it can also be `None` in which case it's no space. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:184 on 2024-02-15 10:58_

Nit: I'm actually less concerned about trailing commas here. Handling them in the builder is an *easy enough* change that only needs to be done in a single place. 

My main concern with using `RemoveSoftlineBreaks` is that we need to handle this case in all expressions and keeping track of it is error prone. That's why I would use a different heuristic here: Only allow expressions that we are *certain* not to insert any new tokens and that don't use any soft line breaks:

* Names: `a`
* Attribute chains: `a.b`
* Literals: `True`, `False`, `None`, `4.5`, ...

This way we can also avoid having to use `RemoveSoftlineBreaks` and the Printer approach.

Or did this change prevent a perf regression?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:225 on 2024-02-15 11:02_

Nit: I would probably not use `write` here to avoid the need for `format_with`


```suggestion
                    token(":").fmt(f)?;
                    f.join()
                        .entries(format_spec.elements.iter().map(|element| {
                            FormatFStringElement::new(element, self.context)
                        }))
                        .finish()?;
                    trailing_comments(trailing_format_spec_comments).fmt(f)?;
                }
```

---

_Comment by @dhruvmanila on 2024-02-15 11:19_

> Is the logic if replacement expressions are allowed to expand over multiple lines globally for the entire f-string-literal (expand if any replacement expression contains a line break), or is the decision made for each replacement expression (only expand the replacement expressions that already contain line breaks, keep the other ones flat)?

The decision is made globally for an entire f-string by looking at each replacement field and checking if _any_ of them contains a line break.

> Does this also apply to Python 3.12 or does Python 3.12 allow to reuse the same triple quotes?

Python 3.12 allows re-use of same quotes irrespective of whether it's single or triple quoted. The logic basically ORs with checking if the target-version is 3.12 or not.

> Does that mean we do not format any f-string that contains any debug expression or is it that we only disable formatting for replacements that contain debug expressions?

The later. So, given `f"a {   x   = } b { y    } c"`, we need to preserve the whitespace around `x` and not for `y`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:249 on 2024-02-15 12:00_

Same here. I don't think we need the indent or group if it should be all flat
```suggestion
                            [
                                dangling_open_parenthesis_comments(
                                    dangling_open_parentheses_comments
                                ),
                                item
                            ]
                        )
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:266 on 2024-02-15 12:11_

I think we can reduce the code duplication here by making use of `Option` as you did above with the spacing. 


```suggestion
            let open_parenthesis_comments = (!dangling_item_comments.is_empty()).then_some(
                dangling_open_parenthesis_comments(dangling_open_parentheses_comments),
            );

            write!(f, [token("{")])?;

            {
                let mut f = WithNodeLevel::new(NodeLevel::ParenthesizedExpression, f);
                if self.context.should_remove_soft_line_breaks() {
                    let mut buffer = RemoveSoftLinesBuffer::new(&mut *f);
                    write!(buffer, [open_parenthesis_comments, item])?;
                } else {
                    group(&format_args![
                        open_parenthesis_comments,
                        soft_block_indent(&item)
                    ])
                    .fmt(&mut f)?;
                }
            }

            token("}").fmt(f)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:156 on 2024-02-15 12:19_

I think it's best if we don't rely on the behavior of `RemoveSoftlinesBuffer` here and explicilty encode whether it should be a line break or a space
```suggestion
                        Some(format_with(|f| {
                            if self.context.should_remove_soft_line_breaks() {
                                space().fmt(f)
                            } else {
                                soft_line_break_or_space().fmt(f)
                            }
                        }))
```

---

_@MichaReiser approved on 2024-02-15 12:29_

Excellent work @dhruvmanila

We can follow up with the indent handling in a separate PR if we decide to change it. 

I have a few comments about the implementations; most are small Nits. 

My main concerns are about combining the `RemoveSoftlinesBuffer` with the `Printer` approach and how trailing comments are now associated as dangling comments. 

I suggest that we either use the `RemoveSoftlinesBuffer` or the `Printer` approach but that we avoid using both to keep things "simple".  If you prefer to keep `RemoveSoftlinesBuffer`, then I would prefer changing the magic trailing comma handling by checking some state in the context over using the `Printer`. It avoids the performance penalty and doesn't require a visitor to determine if it is necessary to use it or not. 

---

_@dhruvmanila reviewed on 2024-02-16 09:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py`:72 on 2024-02-16 09:38_

Good point. I'll also change the other comments while I'm at it.

---

_@dhruvmanila reviewed on 2024-02-16 09:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:299 on 2024-02-16 09:45_

The problem is that the comment will get attached to the last f-string element which could be either `FStringLiteralElement` in the given example or `FStringExpressionElement` like in the following example:

```python
f"{
   foo
   :>{x}
    # comment 27
}"
```

That actually makes me thinking that we can actually use the `FStringFormatSpec` node and it will become a trailing comment to that node if they both (comment and node) are present in the source code. Let me test it out.

---

_@MichaReiser reviewed on 2024-02-16 09:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:299 on 2024-02-16 09:52_

Ideally comments always get associated with the outer-most node, which `FStringFormatSpec` wouldn't be. 

Maybe change the implementation to always associate it with the outermost element (I'm once more confused by the name and don't know what AST nodes they are :confused: )

---

_@dhruvmanila reviewed on 2024-02-16 10:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:299 on 2024-02-16 10:08_

> Ideally comments always get associated with the outer-most node

I see, this actually makes more sense.

---

_@dhruvmanila reviewed on 2024-02-16 10:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:299 on 2024-02-16 10:23_

For future reference the reason I was thinking to attach it to the format spec is because the format spec is optional which means the comment only occurs if format spec is present.

But I still think attaching it as a trailing comment to the expression element makes more sense. I'm going forward with that.

---

_@dhruvmanila reviewed on 2024-02-16 11:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:121 on 2024-02-16 11:05_

Yes, that's correct. The entire f-string will be normalized in one go if preview isn't enabled.

---

_@dhruvmanila reviewed on 2024-02-16 11:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string.rs`:68 on 2024-02-16 11:07_

Yeah, makes sense. I'll do that in a follow-up PR because I believe there are other places, not relevant to this PR, which needs updating as well.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:266 on 2024-02-16 11:39_

Thanks, this simplifies a lot!

---

_@dhruvmanila reviewed on 2024-02-16 11:39_

---

_@dhruvmanila reviewed on 2024-02-16 11:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:235 on 2024-02-16 11:40_

Oh yes, I started with that but somewhere along the middle I added them for no reason. I'm not sure why though.

---

_Comment by @dhruvmanila on 2024-02-16 12:41_

Thanks for the detailed review @MichaReiser, really appreciate all the suggestions you've provided.

Regarding the magic trailing comma, I've removed the `Printer` usage and updated the builder to avoid adding the trailing comma if the f-string layout is flat. We already have that information in the context so it was an easy change.

https://github.com/astral-sh/ruff/blob/5c056a1dee786ee6554c5ac09db2c681ab4ae545/crates/ruff_python_formatter/src/builders.rs#L207-L215

I'll look at the final ecosystem changes and then merge this PR.

---

_@dhruvmanila reviewed on 2024-02-16 12:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:184 on 2024-02-16 12:41_

> Handling them in the builder is an easy enough change that only needs to be done in a single place.

I've gone with this approach. Thanks for the suggestion!

---

_@dhruvmanila reviewed on 2024-02-16 12:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:231 on 2024-02-16 12:42_

This isn't relevant anymore but rust-analyzer was giving me an error and the compiler wasn't. I didn't look into it in detail then.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:216 on 2024-02-16 13:33_

This should be insid eof `self.result.and_then` or it will return `Ok()` even when `self.result` is `Err`

---

_@MichaReiser reviewed on 2024-02-16 13:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:115 on 2024-02-16 13:35_

Is it possible that the element can have leading comments? Or is this even necessary considering that you call `mark_verbatim_node_comments_formatted`

Edit: `mark_verbatim_node_comments_formatted` only marks dangling and inner comments as formatted but not the leading and trailing

---

_@MichaReiser reviewed on 2024-02-16 13:35_

---

_@dhruvmanila reviewed on 2024-02-16 13:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:115 on 2024-02-16 13:36_

No, those will become leading comments to the expression _inside_ the element.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:211 on 2024-02-16 13:36_

Can an element have leading comments that need to be formatted?

---

_@MichaReiser reviewed on 2024-02-16 13:36_

---

_@dhruvmanila reviewed on 2024-02-16 13:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:211 on 2024-02-16 13:38_

No, those will become leading comments to the expression _inside_ the element.

---

_Comment by @dhruvmanila on 2024-02-16 13:57_

_For posterity..._

Currently, to avoid breaking the expression, what we do is after the expression formatting is over, we'd remove the line breaks. This creates a problem w.r.t. the magic trailing comma. For example,

```python
f"aaaaaaa {['aaaaaaaaaaaaaaa', 'bbbbbbbbbbbbb', 'ccccccccccccccccc', 'ddddddddddddddd', 'eeeeeeeeeeeeee']} aaaaaaa"
```

The formatter will break the list but as there were no line breaks in the original source code, we'd collapse it back. The trailing comma will still remain:

```python
f"aaaaaaa {['aaaaaaaaaaaaaaa', 'bbbbbbbbbbbbb', 'ccccccccccccccccc', 'ddddddddddddddd', 'eeeeeeeeeeeeee',]} aaaaaaa"
```

The current approach to solving this problem is to update the builder to not add the trailing comma in this specific context:

https://github.com/astral-sh/ruff/blob/5c056a1dee786ee6554c5ac09db2c681ab4ae545/crates/ruff_python_formatter/src/builders.rs#L207-L215

**An alternative approach**, which is what this comment documents, is to use the `Printer` in the f-string formatting itself and avoid updating the builder. That would also remove the use of `RemoveSoftlineBreak` buffer. This [commit](https://github.com/astral-sh/ruff/pull/9642/commits/5c056a1dee786ee6554c5ac09db2c681ab4ae545) has a version of it which has been removed. There are few more changes which would need to be made along with that:
1. Update the `LineWidth` to use `u32` as `u16` is reasonably large but a line can go beyond that.
2. Remove `RemoveSoftlineBreak` buffer usage as the printer won't add the line breaks in the first place.

This also has an advantage that the logic is local to the f-string formatting.

---

_@dhruvmanila reviewed on 2024-02-16 14:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string.rs`:51 on 2024-02-16 14:16_

What I was thinking was that we could probably reduce the scope of the function to only use the f-string literal elements and not the expression elements in 3.12. The reason being that same quotes can be re-used in 3.12. But, I think the current logic is correct, I'll remove the comment. It might be more involved to check if the scope can be reduced.

---

_Merged by @dhruvmanila on 2024-02-16 14:58_

---

_Closed by @dhruvmanila on 2024-02-16 14:58_

---

_Branch deleted on 2024-02-16 14:58_

---
