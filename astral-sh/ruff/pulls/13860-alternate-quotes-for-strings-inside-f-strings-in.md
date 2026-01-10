```yaml
number: 13860
title: Alternate quotes for strings inside f-strings in preview 
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/f-string-alternate-quotes
created_at: 2024-10-21T14:28:24Z
updated_at: 2024-10-23T05:57:55Z
url: https://github.com/astral-sh/ruff/pull/13860
synced_at: 2026-01-10T20:59:37Z
```

# Alternate quotes for strings inside f-strings in preview 

---

_Pull request opened by @MichaReiser on 2024-10-21 14:28_

## Summary

This PR changes how quotes are selected for f-strings and nested string literals when preview mode is on.

For the outer f-string: The logic is the same as for any other strings, except that we only count the quotes inside literal parts. This is different from before where `choose_quotes` analyzed the entire f-string, including expressions. 

The rules for inner f-strings are:

* If the outer f-string is a triple quoted string: Prefer the configured quote style 
* If the outer f-string is single quoted: Prefer the opposite quote style (alternate the quote styles).

```python
f"{'inner'}"

# Before
f"{"inner"}"
```

The only exception to the above rule is when targeting <Py312 and the f-string contains a debug-expression that itself contains quotes:

```python
f'{a + "b"=}' # Can't change the quotes here
```

In this case, changing the quotes is unsafe because the debug expression will retain its existing quotes. That's why the implementation must preserve the outer quotes as is. Changing the outer quotes is fine when targeting Py312+.

```python
f"{a + "b"=}"
```

The advantage of limiting the pre-312 behavior to only debug expressions is that an upgrade of the target version to 312 should result in a very small diff considering that debug expressions containing quotes should be rare.


Fixes https://github.com/astral-sh/ruff/issues/13237
Fixes https://github.com/astral-sh/ruff/issues/11056

## Considerations

This new style no longer optimizes for the fewest "foreign" quotes in total. E.g. it changes

```python
return f'#{data["node"]["number"]} {component_str}{data["node"]["title"]}'
```

to 

```python
return f"#{data['node']['number']} {component_str}{data['node']['title']}"
```

where the total preferred quotes count is 2, but it uses 8 single quotes.

## Test Plan

* Added tests
* Reviewed the snapshot changes
* Reviewed the ecosystem changes

## Review

The most useful for review is if you can think of more cursed f-string examples where the above approach might produce invalid f-strings or weird looking quote selections.


---

_Label `formatter` added by @MichaReiser on 2024-10-21 14:28_

---

_Label `preview` added by @MichaReiser on 2024-10-21 14:28_

---

_@MichaReiser reviewed on 2024-10-21 14:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:247 on 2024-10-21 14:48_

Both `from_part` and merge are also needed for implicit string formatting. We get good reuse :)

---

_Comment by @github-actions[bot] on 2024-10-21 15:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+338 -338 lines in 178 files in 30 projects; 24 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/opus.py#L371'>disnake/opus.py~L371</a>
```diff
     def set_bandwidth(self, req: BAND_CTL) -> None:
         if req not in band_ctl:
             raise KeyError(
-                f'{req!r} is not a valid bandwidth setting. Try one of: {",".join(band_ctl)}'
+                f"{req!r} is not a valid bandwidth setting. Try one of: {','.join(band_ctl)}"
             )
 
         k = band_ctl[req]
```
<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/opus.py#L380'>disnake/opus.py~L380</a>
```diff
     def set_signal_type(self, req: SIGNAL_CTL) -> None:
         if req not in signal_ctl:
             raise KeyError(
-                f'{req!r} is not a valid bandwidth setting. Try one of: {",".join(signal_ctl)}'
+                f"{req!r} is not a valid bandwidth setting. Try one of: {','.join(signal_ctl)}"
             )
 
         k = signal_ctl[req]
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L346'>rasa/core/training/interactive.py~L346</a>
```diff
     choices = []
     for p in sorted_intents:
         name_with_confidence = (
-            f'{p.get("confidence"):03.2f} {p.get(INTENT_NAME_KEY):40}'
+            f"{p.get('confidence'):03.2f} {p.get(INTENT_NAME_KEY):40}"
         )
         choice = {
             INTENT_NAME_KEY: name_with_confidence,
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L674'>rasa/core/training/interactive.py~L674</a>
```diff
     await _print_history(conversation_id, endpoint)
 
     choices = [
-        {"name": f'{a["score"]:03.2f} {a["action"]:40}', "value": a["action"]}
+        {"name": f"{a['score']:03.2f} {a['action']:40}", "value": a["action"]}
         for a in predictions
     ]
 
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/1acca459e210e7a40513d7663108c3e873b2f85b/tests/test_connection.py#L1268'>tests/test_connection.py~L1268</a>
```diff
             ["connection", "generate-jwt", "-c", "jwt"],
         )
         assert (
-            f'{attribute.capitalize().replace("_", " ")} is not set in the connection context'
+            f"{attribute.capitalize().replace('_', ' ')} is not set in the connection context"
             in result.output
         )
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -8 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2035'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py~L2035</a>
```diff
     """Check if package has been prepared in dist folder."""
     return any(
         file.startswith((
-            f'apache_airflow_providers_{package.replace(".", "_")}',
-            f'apache-airflow-providers-{package.replace(".", "-")}',
+            f"apache_airflow_providers_{package.replace('.', '_')}",
+            f"apache-airflow-providers-{package.replace('.', '-')}",
         ))
         for file in dist_files
     )
```
<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2048'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py~L2048</a>
```diff
 def get_suffix_from_package_in_dist(dist_files: list[str], package: str) -> str | None:
     """Get suffix from package prepared in dist folder."""
     for file in dist_files:
-        if file.startswith(f'apache_airflow_providers_{package.replace(".", "_")}') and file.endswith(
+        if file.startswith(f"apache_airflow_providers_{package.replace('.', '_')}") and file.endswith(
             ".tar.gz"
         ):
             file = file[: -len(".tar.gz")]
```
<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/dev/breeze/src/airflow_breeze/utils/packages.py#L714'>dev/breeze/src/airflow_breeze/utils/packages.py~L714</a>
```diff
             f"[error]Error {e}[/]\n"
             f"[error]When fetching tags from remote. Your tags might not be refreshed.[/]\n\n"
             f'[warning]Please refresh the tags manually via:[/]\n\n"'
-            f'{" ".join(fetch_command)}\n\n'
+            f"{' '.join(fetch_command)}\n\n"
         )
         sys.exit(1)
 
```
<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/providers/src/airflow/providers/docker/decorators/docker.py#L161'>providers/src/airflow/providers/docker/decorators/docker.py~L161</a>
```diff
             f"""bash -cx  '{
                 _generate_decode_command("__PYTHON_SCRIPT", "/tmp/script.py", self.python_command)
             } &&"""
-            f'{_generate_decode_command("__PYTHON_INPUT", "/tmp/script.in", self.python_command)} &&'
+            f"{_generate_decode_command('__PYTHON_INPUT', '/tmp/script.in', self.python_command)} &&"
             f"{self.python_command} /tmp/script.py /tmp/script.in /tmp/script.out none /tmp/script.out'"
         )
 
```
<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/providers/tests/system/amazon/aws/example_sagemaker_endpoint.py#L211'>providers/tests/system/amazon/aws/example_sagemaker_endpoint.py~L211</a>
```diff
     upload_data = S3CreateObjectOperator(
         task_id="upload_data",
         s3_bucket=test_setup["bucket_name"],
-        s3_key=f'{test_setup["input_data_s3_key"]}/train.csv',
+        s3_key=f"{test_setup['input_data_s3_key']}/train.csv",
         data=TRAIN_DATA,
     )
 
```
<a href='https://github.com/apache/airflow/blob/35142a907f5f37c9a4d0fb2f402831703f258ad3/tests/models/test_dagbag.py#L498'>tests/models/test_dagbag.py~L498</a>
```diff
         dag_id = expected_dag.dag_id
         actual_dagbag.log.info("validating %s", dag_id)
         assert (dag_id in actual_found_dag_ids) == should_be_found, (
-            f"dag \"{dag_id}\" should {'' if should_be_found else 'not '}"
+            f'dag "{dag_id}" should {"" if should_be_found else "not "}'
             f'have been found after processing dag "{expected_dag.dag_id}"'
         )
         assert (dag_id in actual_dagbag.dags) == should_be_found, (
-            f"dag \"{dag_id}\" should {'' if should_be_found else 'not '}"
+            f'dag "{dag_id}" should {"" if should_be_found else "not "}'
             f'be in dagbag.dags after processing dag "{expected_dag.dag_id}"'
         )
 
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/superset/db_engine_specs/snowflake.py#L334'>superset/db_engine_specs/snowflake.py~L334</a>
```diff
         if missing := sorted(required - present):
             errors.append(
                 SupersetError(
-                    message=f'One or more parameters are missing: {", ".join(missing)}',
+                    message=f"One or more parameters are missing: {', '.join(missing)}",
                     error_type=SupersetErrorType.CONNECTION_MISSING_PARAMETERS_ERROR,
                     level=ErrorLevel.WARNING,
                     extra={"missing": missing},
```
<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/superset/sql_lab.py#L267'>superset/sql_lab.py~L267</a>
```diff
         if not query.tmp_table_name:
             start_dttm = datetime.fromtimestamp(query.start_time)
             query.tmp_table_name = (
-                f'tmp_{query.user_id}_table_{start_dttm.strftime("%Y_%m_%d_%H_%M_%S")}'
+                f"tmp_{query.user_id}_table_{start_dttm.strftime('%Y_%m_%d_%H_%M_%S')}"
             )
         sql = parsed_query.as_create_table(
             query.tmp_table_name,
```
<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/superset/tags/api.py#L143'>superset/tags/api.py~L143</a>
```diff
         """Deterministic string representation of the API instance for etag_cache."""
         return (
             "Superset.tags.api.TagRestApi@v"
-            f'{self.appbuilder.app.config["VERSION_STRING"]}'
-            f'{self.appbuilder.app.config["VERSION_SHA"]}'
+            f"{self.appbuilder.app.config['VERSION_STRING']}"
+            f"{self.appbuilder.app.config['VERSION_SHA']}"
         )
 
     @expose("/", methods=("POST",))
```
<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/tests/integration_tests/datasets/api_tests.py#L723'>tests/integration_tests/datasets/api_tests.py~L723</a>
```diff
 
         # cleanup
         data = json.loads(rv.data.decode("utf-8"))
-        uri = f'api/v1/dataset/{data.get("id")}'
+        uri = f"api/v1/dataset/{data.get('id')}"
         rv = self.client.delete(uri)
         assert rv.status_code == 200
         with example_db.get_sqla_engine() as engine:
```
<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/tests/integration_tests/datasets/api_tests.py#L808'>tests/integration_tests/datasets/api_tests.py~L808</a>
```diff
 
             # cleanup
             data = json.loads(rv.data.decode("utf-8"))
-            uri = f'api/v1/dataset/{data.get("id")}'
+            uri = f"api/v1/dataset/{data.get('id')}"
             rv = self.client.delete(uri)
             assert rv.status_code == 200
 
```
<a href='https://github.com/apache/superset/blob/4433ef47feccb4bdac00ea9235802202b1e92952/tests/integration_tests/datasource_tests.py#L541'>tests/integration_tests/datasource_tests.py~L541</a>
```diff
 
     sql = (
         f"select * from ({virtual_dataset.sql}) as tbl "
-        f'limit {app.config["SAMPLES_ROW_LIMIT"]}'
+        f"limit {app.config['SAMPLES_ROW_LIMIT']}"
     )
     eager_samples = virtual_dataset.database.get_df(sql)
 
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+2 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/97e63dcc2738529eded8eecfef4b875abc3a476f/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L9'>tests/integration/testdata/remote_invoke/lambda-fns/main.py~L9</a>
```diff
     print("value2 = " + event["key2"])
     print("value3 = " + event["key3"])
 
-    return {"message": f'{event["key1"]} {event["key3"]}'}
+    return {"message": f"{event['key1']} {event['key3']}"}
 
 
 def custom_env_var_echo_handler(event, context):
```
<a href='https://github.com/aws/aws-sam-cli/blob/97e63dcc2738529eded8eecfef4b875abc3a476f/tests/unit/commands/pipeline/init/test_initeractive_init_flow.py#L188'>tests/unit/commands/pipeline/init/test_initeractive_init_flow.py~L188</a>
```diff
             str(["2", "pipeline_execution_role"]): "arn:aws:iam::123456789012:role/execution-role",
             str(["default", "pipeline_execution_role"]): "arn:aws:iam::123456789012:role/execution-role",
             str(["stage_names_message"]): "Here are the stage configuration names detected "
-            f'in {os.path.join(".aws-sam", "pipeline", "pipelineconfig.toml")}:\n\t1 - testing\n\t2 - prod',
+            f"in {os.path.join('.aws-sam', 'pipeline', 'pipelineconfig.toml')}:\n\t1 - testing\n\t2 - prod",
             "shared_values": "default",
         })
         cookiecutter_mock.assert_called_once_with(
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+12 -12 lines across 8 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/Conversation_To_File.py#L246'>crazy_functions/Conversation_To_File.py~L246</a>
```diff
         local_history = "<br/>".join([
             "`" + hide_cwd(f) + f" ({gen_file_preview(f)})" + "`"
             for f in glob.glob(
-                f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+                f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
                 recursive=True,
             )
         ])
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/Conversation_To_File.py#L285'>crazy_functions/Conversation_To_File.py~L285</a>
```diff
     local_history = "<br/>".join([
         "`" + hide_cwd(f) + "`"
         for f in glob.glob(
-            f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+            f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
             recursive=True,
         )
     ])
     for f in glob.glob(
-        f'{get_log_folder(get_user(chatbot), plugin_name="chat_history")}/**/{f_prefix}*.html',
+        f"{get_log_folder(get_user(chatbot), plugin_name='chat_history')}/**/{f_prefix}*.html",
         recursive=True,
     ):
         os.remove(f)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/Markdown_Translate.py#L193'>crazy_functions/Markdown_Translate.py~L193</a>
```diff
                 txt = txt.replace("/blob/", "/")
 
         r = requests.get(txt, proxies=proxies)
-        download_local = f'{get_log_folder(plugin_name="批量Markdown翻译")}/raw-readme-{gen_time_str()}.md'
-        project_folder = f'{get_log_folder(plugin_name="批量Markdown翻译")}'
+        download_local = f"{get_log_folder(plugin_name='批量Markdown翻译')}/raw-readme-{gen_time_str()}.md"
+        project_folder = f"{get_log_folder(plugin_name='批量Markdown翻译')}"
         with open(download_local, "wb+") as f:
             f.write(r.content)
         file_manifest = [download_local]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/Social_Helper.py#L182'>crazy_functions/Social_Helper.py~L182</a>
```diff
 
         try:
             Explaination = "\n".join([
-                f'{k}: {v["explain_to_llm"]}' for k, v in self.tools_to_select.items()
+                f"{k}: {v['explain_to_llm']}" for k, v in self.tools_to_select.items()
             ])
 
             class UserSociaIntention(BaseModel):
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/gen_fns/gen_fns_shared.py#L22'>crazy_functions/gen_fns/gen_fns_shared.py~L22</a>
```diff
 
 def try_make_module(code, chatbot):
     module_file = "gpt_fn_" + gen_time_str().replace("-", "_")
-    fn_path = f'{get_log_folder(plugin_name="gen_plugin_verify")}/{module_file}.py'
+    fn_path = f"{get_log_folder(plugin_name='gen_plugin_verify')}/{module_file}.py"
     with open(fn_path, "w", encoding="utf8") as f:
         f.write(code)
     promote_file_to_downloadzone(fn_path, chatbot=chatbot)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/gen_fns/gen_fns_shared.py#L70'>crazy_functions/gen_fns/gen_fns_shared.py~L70</a>
```diff
     return_dict["traceback"] = ""
     try:
         module_file = "gpt_fn_" + gen_time_str().replace("-", "_")
-        fn_path = f'{get_log_folder(plugin_name="gen_plugin_run")}/{module_file}.py'
+        fn_path = f"{get_log_folder(plugin_name='gen_plugin_run')}/{module_file}.py"
         with open(fn_path, "w", encoding="utf8") as f:
             f.write(code)
         class_name = get_class_name(code)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/vt_fns/vt_call_plugin.py#L187'>crazy_functions/vt_fns/vt_call_plugin.py~L187</a>
```diff
     # ⭐ ⭐ ⭐ 执行插件
     fn = plugin["Function"]
     fn_name = fn.__name__
-    msg = f'{llm_kwargs["llm_model"]}为您选择了插件: `{fn_name}`\n\n插件说明：{plugin["Info"]}\n\n插件参数：{plugin_sel.plugin_arg}\n\n假如偏离了您的要求，按停止键终止。'
+    msg = f"{llm_kwargs['llm_model']}为您选择了插件: `{fn_name}`\n\n插件说明：{plugin['Info']}\n\n插件参数：{plugin_sel.plugin_arg}\n\n假如偏离了您的要求，按停止键终止。"
     yield from update_ui_lastest_msg(
         lastmsg=msg, chatbot=chatbot, history=history, delay=2
     )
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/request_llms/bridge_all.py#L1170'>request_llms/bridge_all.py~L1170</a>
```diff
             raise ValueError("AZURE_CFG_ARRAY中配置的模型必须以azure开头")
         endpoint_ = (
             azure_cfg_dict["AZURE_ENDPOINT"]
-            + f'openai/deployments/{azure_cfg_dict["AZURE_ENGINE"]}/chat/completions?api-version=2023-05-15'
+            + f"openai/deployments/{azure_cfg_dict['AZURE_ENGINE']}/chat/completions?api-version=2023-05-15"
         )
         model_info.update({
             azure_model_name: {
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/request_llms/com_google.py#L101'>request_llms/com_google.py~L101</a>
```diff
 def html_local_file(file):
     base_path = os.path.dirname(__file__)  # 项目目录
     if os.path.exists(str(file)):
-        file = f'file={file.replace(base_path, ".")}'
+        file = f"file={file.replace(base_path, '.')}"
     return file
 
 
```
<a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/shared_utils/handle_upload.py#L14'>shared_utils/handle_upload.py~L14</a>
```diff
 def html_local_file(file):
     base_path = os.path.dirname(__file__)  # 项目目录
     if os.path.exists(str(file)):
-        file = f'file={file.replace(base_path, ".")}'
+        file = f"file={file.replace(base_path, '.')}"
     return file
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L67'>scripts/milestone.py~L67</a>
```diff
     """Returns a humanized description of the given issue or PR data."""
     component = get_label_component(data)
     component_str = "" if not component else f"[component: {component}] "
-    return f'#{data["node"]["number"]} {component_str}{data["node"]["title"]}'
+    return f"#{data['node']['number']} {component_str}{data['node']['title']}"
 
 
 def get_milestone_number(title, token, allow_closed):
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+3 -3 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/blinksync.py#L65'>blinksync/blinksync.py~L65</a>
```diff
             await my_sync.refresh()
             if my_sync.local_storage and my_sync.local_storage_manifest_ready:
                 print("Manifest is ready")
-                print(f"Manifest {my_sync._local_storage["manifest"]}")
+                print(f"Manifest {my_sync._local_storage['manifest']}")
             else:
                 print("Manifest not ready")
             for name, camera in blink.cameras.items():
```
<a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/blinksync.py#L91'>blinksync/blinksync.py~L91</a>
```diff
                         await item.prepare_download(blink)
                         await item.download_video(
                             blink,
-                            f"{path}/{item.name}_{item.created_at.astimezone().isoformat().replace(":", "_")}.mp4",
+                            f"{path}/{item.name}_{item.created_at.astimezone().isoformat().replace(':', '_')}.mp4",
                         )
                     if button == DELETE:
                         await item.delete_video(blink)
```
<a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/tests/test_camera_functions.py#L119'>tests/test_camera_functions.py~L119</a>
```diff
                     f"temperature response {mock_resp.return_value}."
                 ),
                 (
-                    f"WARNING:blinkpy.camera:for network_id ({config["network_id"]}) "
+                    f"WARNING:blinkpy.camera:for network_id ({config['network_id']}) "
                     f"and camera_id ({self.camera.camera_id})"
                 ),
                 ("WARNING:blinkpy.camera:Could not find thumbnail for camera new."),
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/3252b38ce372427ad3913e894a66f05b153443c1/ibis/backends/clickhouse/tests/test_client.py#L362'>ibis/backends/clickhouse/tests/test_client.py~L362</a>
```diff
 
 
 def test_password_with_bracket():
-    password = f'{os.environ.get("IBIS_TEST_CLICKHOUSE_PASSWORD", "")}[]'
+    password = f"{os.environ.get('IBIS_TEST_CLICKHOUSE_PASSWORD', '')}[]"
     quoted_pass = quote_plus(password)
     host = os.environ.get("IBIS_TEST_CLICKHOUSE_HOST", "localhost")
     user = os.environ.get("IBIS_TEST_CLICKHOUSE_USER", "default")
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+16 -16 lines across 6 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/docs/docs/how_to/chat_token_usage_tracking.ipynb#L138'>docs/docs/how_to/chat_token_usage_tracking.ipynb~L138</a>
```diff
     }
    ],
    "source": [
-    "print(f'OpenAI: {openai_response.response_metadata[\"token_usage\"]}\\n')\n",
-    "print(f'Anthropic: {anthropic_response.response_metadata[\"usage\"]}')"
+    "print(f\"OpenAI: {openai_response.response_metadata['token_usage']}\\n\")\n",
+    "print(f\"Anthropic: {anthropic_response.response_metadata['usage']}\")"
    ]
   },
   {
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/docs/docs/how_to/chat_token_usage_tracking.ipynb#L307'>docs/docs/how_to/chat_token_usage_tracking.ipynb~L307</a>
```diff
     "\n",
     "async for event in structured_llm.astream_events(\"Tell me a joke\", version=\"v2\"):\n",
     "    if event[\"event\"] == \"on_chat_model_end\":\n",
-    "        print(f'Token usage: {event[\"data\"][\"output\"].usage_metadata}\\n')\n",
+    "        print(f\"Token usage: {event['data']['output'].usage_metadata}\\n\")\n",
     "    elif event[\"event\"] == \"on_chain_end\":\n",
     "        print(event[\"data\"][\"output\"])\n",
     "    else:\n",
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/docs/docs/how_to/message_history.ipynb#L376'>docs/docs/how_to/message_history.ipynb~L376</a>
```diff
    "source": [
     "state = app.get_state(config).values\n",
     "\n",
-    "print(f'Language: {state[\"language\"]}')\n",
+    "print(f\"Language: {state['language']}\")\n",
     "for message in state[\"messages\"]:\n",
     "    message.pretty_print()"
    ]
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/docs/docs/how_to/message_history.ipynb#L427'>docs/docs/how_to/message_history.ipynb~L427</a>
```diff
    "source": [
     "state = app.get_state(config).values\n",
     "\n",
-    "print(f'Language: {state[\"language\"]}')\n",
+    "print(f\"Language: {state['language']}\")\n",
     "for message in state[\"messages\"]:\n",
     "    message.pretty_print()"
    ]
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/cli/langchain_cli/namespaces/app.py#L249'>libs/cli/langchain_cli/namespaces/app.py~L249</a>
```diff
 
     chain_names = []
     for e in installed_exports:
-        original_candidate = f'{e["package_name"].replace("-", "_")}_chain'
+        original_candidate = f"{e['package_name'].replace('-', '_')}_chain"
         candidate = original_candidate
         i = 2
         while candidate in chain_names:
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L178'>libs/community/langchain_community/graphs/neo4j_graph.py~L178</a>
```diff
                         example = (
                             (
                                 "Available options: "
-                                f'{[clean_string_values(el) for el in prop["values"]]}'
+                                f"{[clean_string_values(el) for el in prop['values']]}"
                             )
                             if prop["values"]
                             else ""
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L192'>libs/community/langchain_community/graphs/neo4j_graph.py~L192</a>
```diff
                     "LOCAL_DATE_TIME",
                 ]:
                     if prop.get("min") is not None:
-                        example = f'Min: {prop["min"]}, Max: {prop["max"]}'
+                        example = f"Min: {prop['min']}, Max: {prop['max']}"
                     else:
                         example = (
                             f'Example: "{prop["values"][0]}"'
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L204'>libs/community/langchain_community/graphs/neo4j_graph.py~L204</a>
```diff
                     if not prop.get("min_size") or prop["min_size"] > LIST_LIMIT:
                         continue
                     example = (
-                        f'Min Size: {prop["min_size"]}, Max Size: {prop["max_size"]}'
+                        f"Min Size: {prop['min_size']}, Max Size: {prop['max_size']}"
                     )
                 formatted_node_props.append(
                     f"  - `{prop['property']}`: {prop['type']} {example}"
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L226'>libs/community/langchain_community/graphs/neo4j_graph.py~L226</a>
```diff
                         example = (
                             (
                                 "Available options: "
-                                f'{[clean_string_values(el) for el in prop["values"]]}'
+                                f"{[clean_string_values(el) for el in prop['values']]}"
                             )
                             if prop["values"]
                             else ""
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L239'>libs/community/langchain_community/graphs/neo4j_graph.py~L239</a>
```diff
                     "LOCAL_DATE_TIME",
                 ]:
                     if prop.get("min"):  # If we have min/max
-                        example = f'Min: {prop["min"]}, Max:  {prop["max"]}'
+                        example = f"Min: {prop['min']}, Max:  {prop['max']}"
                     else:  # return a single value
                         example = (
                             f'Example: "{prop["values"][0]}"' if prop["values"] else ""
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/graphs/neo4j_graph.py#L249'>libs/community/langchain_community/graphs/neo4j_graph.py~L249</a>
```diff
                     if not prop.get("min_size") or prop["min_size"] > LIST_LIMIT:
                         continue
                     example = (
-                        f'Min Size: {prop["min_size"]}, Max Size: {prop["max_size"]}'
+                        f"Min Size: {prop['min_size']}, Max Size: {prop['max_size']}"
                     )
                 formatted_rel_props.append(
                     f"  - `{prop['property']}: {prop['type']}` {example}"
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/libs/community/langchain_community/vectorstores/typesense.py#L159'>libs/community/langchain_community/vectorstores/typesense.py~L159</a>
```diff
         embedded_query = [str(x) for x in self._embedding.embed_query(query)]
         query_obj = {
             "q": "*",
-            "vector_query": f'vec:([{",".join(embedded_query)}], k:{k})',
+            "vector_query": f"vec:([{','.join(embedded_query)}], k:{k})",
             "filter_by": filter,
             "collection": self._typesense_collection_name,
         }
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L78'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L78</a>
```diff
         try:
             return (
                 "Recommended movies are: "
-                f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
+                f"{f'###Movie {nl}'.join([el['movie'] for el in response])}"
             )
         except Exception:
             return "Can you tell us about some of the movies you liked?"
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L88'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L88</a>
```diff
         try:
             return (
                 "Recommended movies are: "
-                f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
+                f"{f'###Movie {nl}'.join([el['movie'] for el in response])}"
             )
         except Exception:
             return "Something went wrong"
```
<a href='https://github.com/langchain-ai/langchain/blob/0640cbf2f126f773b7ae78b0f94c1ba0caabb2c1/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L102'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L102</a>
```diff
     try:
         return (
             "Recommended movies are: "
-            f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
+            f"{f'###Movie {nl}'.join([el['movie'] for el in response])}"
         )
     except Exception:
         return "Something went wrong"
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/06fe2bf81530454968297f225ab382490139c145/latch_cli/services/move.py#L156'>latch_cli/services/move.py~L156</a>
```diff
                 raise e
 
     if len(srcs) == 1:
-        src_str = f'{click.style("Source: ", fg="blue")}{srcs[0]}'
+        src_str = f"{click.style('Source: ', fg='blue')}{srcs[0]}"
     else:
         src_str = "\n".join([
             click.style("Sources: ", fg="blue"),
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+7 -7 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/wallets/phoenixd.py#L232'>lnbits/wallets/phoenixd.py~L232</a>
```diff
                             and message_json.get("type") == "payment_received"
                         ):
                             logger.info(
-                                f'payment-received: {message_json["paymentHash"]}'
+                                f"payment-received: {message_json['paymentHash']}"
                             )
                             yield message_json["paymentHash"]
 
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L45'>tests/regtest/test_real_invoice.py~L45</a>
```diff
 
     # check the payment status
     response = await client.get(
-        f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+        f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
     )
     assert response.status_code < 300
     payment_status = response.json()
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L74'>tests/regtest/test_real_invoice.py~L74</a>
```diff
     invoice = response.json()
 
     response = await client.get(
-        f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+        f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
     )
     assert response.status_code < 300
     payment_status = response.json()
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L84'>tests/regtest/test_real_invoice.py~L84</a>
```diff
         assert payment.checking_id == invoice["payment_hash"]
 
         response = await client.get(
-            f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+            f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
         )
         assert response.status_code < 300
         payment_status = response.json()
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L128'>tests/regtest/test_real_invoice.py~L128</a>
```diff
 
     # check the payment status
     response = await client.get(
-        f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+        f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
     )
     payment_status = response.json()
     assert payment_status["paid"]
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L290'>tests/regtest/test_real_invoice.py~L290</a>
```diff
     assert response.status_code < 300
     invoice = response.json()
     response = await client.get(
-        f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+        f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
     )
     payment_status = response.json()
     assert not payment_status["paid"]
```
<a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/regtest/test_real_invoice.py#L299'>tests/regtest/test_real_invoice.py~L299</a>
```diff
         assert payment.checking_id == invoice["payment_hash"]
 
         response = await client.get(
-            f'/api/v1/payments/{invoice["payment_hash"]}', headers=inkey_headers_from
+            f"/api/v1/payments/{invoice['payment_hash']}", headers=inkey_headers_from
         )
         assert response.status_code < 300
         payment_status = response.json()
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/0fc6b28fcfafa625c53f8f257f39c08ae8e40b88/examples/hello_hybrid_bm25.py#L157'>examples/hello_hybrid_bm25.py~L157</a>
```diff
         print(f"text: {hit.text} distance {hit.score}")
 else:
     for hit in res:
-        print(f'text: {hit.fields["text"]} distance {hit.distance}')
+        print(f"text: {hit.fields['text']} distance {hit.distance}")
 
 # If you used both BGE-M3 and the reranker, you should see the following:
 # text: Alan Turing was the first person to conduct substantial research in AI. distance 0.9306981017573297
```
<a href='https://github.com/milvus-io/pymilvus/blob/0fc6b28fcfafa625c53f8f257f39c08ae8e40b88/examples/hello_hybrid_sparse_dense.py#L151'>examples/hello_hybrid_sparse_dense.py~L151</a>
```diff
         print(f"text: {hit.text} distance {hit.score}")
 else:
     for hit in res:
-        print(f'text: {hit.fields["text"]} distance {hit.distance}')
+        print(f"text: {hit.fields['text']} distance {hit.distance}")
 
 # If you used both BGE-M3 and the reranker, you should see the following:
 # text: Alan Turing was the first person to conduct substantial research in AI. distance 0.9306981017573297
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -3 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/ab44d3b69294fd75bfd7b5d06e1d517355df0a69/tests/recipes/test_ingest_step.py#L103'>tests/recipes/test_ingest_step.py~L103</a>
```diff
     pandas_df_part2 = pandas_df[1:]
     pandas_df_part1.to_csv(dataset_path / "df1.csv")
     pandas_df_part2.to_csv(dataset_path / "df2.csv")
-    dataset_path = [f'{dataset_path / "df1.csv"}', f'{dataset_path / "df2.csv"}']
+    dataset_path = [f"{dataset_path / 'df1.csv'}", f"{dataset_path / 'df2.csv'}"]
 
     recipe_yaml = tmp_recipe_root_path.joinpath(_RECIPE_CONFIG_FILE_NAME)
     recipe_yaml.write_text(
```
<a href='https://github.com/mlflow/mlflow/blob/ab44d3b69294fd75bfd7b5d06e1d517355df0a69/tests/recipes/test_ingest_step.py#L158'>tests/recipes/test_ingest_step.py~L158</a>
```diff
         dataset_path = pathlib.Path(os.path.relpath(dataset_path))
 
     if explicit_file_list and multiple_files:
-        dataset_path = [f'{dataset_path / "df1.csv"}', f'{dataset_path / "df2.csv"}']
+        dataset_path = [f"{dataset_path / 'df1.csv'}", f"{dataset_path / 'df2.csv'}"]
     else:
         dataset_path = str(dataset_path)
 
```
<a href='https://github.com/mlflow/mlflow/blob/ab44d3b69294fd75bfd7b5d06e1d517355df0a69/tests/server/auth/test_auth.py#L114'>tests/server/auth/test_auth.py~L114</a>
```diff
 
     # authenticate with the newly created user
     headers = {
-        "Authorization": f'Bearer {jwt.encode({"username": username}, "secret", algorithm="HS256")}'
+        "Authorization": f"Bearer {jwt.encode({'username': username}, 'secret', algorithm='HS256')}"
     }
     _mlflow_search_experiments_rest(client.tracking_uri, headers)
 
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -6 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/68d9dcab5b543adb3bfe5b83563c61a9b8afae77/pandas/io/formats/xml.py#L404'>pandas/io/formats/xml.py~L404</a>
```diff
                         f"{self.prefix} is not included in namespaces"
                     ) from err
             elif "" in self.namespaces:
-                uri = f'{{{self.namespaces[""]}}}'
+                uri = f"{{{self.namespaces['']}}}"
             else:
                 uri = ""
 
```
<a href='https://github.com/pandas-dev/pandas/blob/68d9dcab5b543adb3bfe5b83563c61a9b8afae77/pandas/io/formats/xml.py#L502'>pandas/io/formats/xml.py~L502</a>
```diff
                         f"{self.prefix} is not included in namespaces"
                     ) from err
             elif "" in self.namespaces:
-                uri = f'{{{self.namespaces[""]}}}'
+                uri = f"{{{self.namespaces['']}}}"
             else:
                 uri = ""
 
```
<a href='https://github.com/pandas-dev/pandas/blob/68d9dcab5b543adb3bfe5b83563c61a9b8afae77/pandas/tests/io/json/test_pandas.py#L1776'>pandas/tests/io/json/test_pandas.py~L1776</a>
```diff
     )
     def test_read_json_with_very_long_file_path(self, compression):
         # GH 46718
-        long_json_path = f'{"a" * 1000}.json{compression}'
+        long_json_path = f"{'a' * 1000}.json{compression}"
         with pytest.raises(
             FileNotFoundError, match=f"File {long_json_path} does not exist"
         ):
```
<a href='https://github.com/pandas-dev/pandas/blob/68d9dcab5b543adb3bfe5b83563c61a9b8afae77/scripts/validate_docstrings.py#L368'>scripts/validate_docstrings.py~L368</a>
```diff
         )
         for err_code in actual_failures - expected_failures:
             sys.stdout.write(
-                f'{prefix}{res["file"]}:{res["file_line"]}:'
+                f"{prefix}{res['file']}:{res['file_line']}:"
                 f"{err_code}:{func_name}:{error_messages[err_code]}\n"
             )
             exit_status += 1
         for err_code in ignore_errors.get(func_name, set()) - actual_failures:
             sys.stdout.write(
-                f'{prefix}{res["file"]}:{res["file_line"]}:'
+                f"{prefix}{res['file']}:{res['file_line']}:"
                 f"{err_code}:{func_name}:"
                 "EXPECTED TO FAIL, BUT NOT FAILING\n"
             )
```
<a href='https://github.com/pandas-dev/pandas/blob/68d9dcab5b543adb3bfe5b83563c61a9b8afae77/scripts/validate_docstrings.py#L407'>scripts/validate_docstrings.py~L407</a>
```diff
 
     sys.stderr.write(header("Validation"))
     if result["errors"]:
-        sys.stderr.write(f'{len(result["errors"])} Errors found for `{func_name}`:\n')
+        sys.stderr.write(f"{len(result['errors'])} Errors found for `{func_name}`:\n")
         for err_code, err_desc in result["errors"]:
             sys.stderr.write(f"\t{err_code}\t{err_desc}\n")
     else:
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/prefecthq/prefect/blob/d09ef03bef4d2ffa70b8f9f260418ecbbf0926a1/src/prefect/blocks/core.py#L623'>src/prefect/blocks/core.py~L623</a>
```diff
         qualified_name = to_qualified_name(cls)
         module_str = ".".join(qualified_name.split(".")[:-1])
         class_name = cls.__name__
-        block_variable_name = f'{cls.get_block_type_slug().replace("-", "_")}_block'
+        block_variable_name = f"{cls.get_block_type_slug().replace('-', '_')}_block"
 
         return dedent(
             f"""\
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/build/blob/84e649587cb13220d6dd74c870e2bc639f608fde/src/build/__main__.py#L323'>src/build/__main__.py~L323</a>
```diff
         '--version',
         '-V',
         action='version',
-        version=f"build {build.__version__} ({','.join(build.__path__)})",
+        version=f'build {build.__version__} ({",".join(build.__path__)})',
     )
     parser.add_argument(
         '--verbose',
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pypa/cibuildwheel/blob/b98602705f5449b8e221a1849a8dc18e8d8b14db/test/utils.py#L293'>test/utils.py~L293</a>
```diff
 
             if machine_arch == "arm64":
                 arm64_macosx = _floor_macosx(min_macosx, "11.0")
-                platform_tags = [f'macosx_{arm64_macosx.replace(".", "_")}_arm64']
+                platform_tags = [f"macosx_{arm64_macosx.replace('.', '_')}_arm64"]
             else:
-                platform_tags = [f'macosx_{min_macosx.replace(".", "_")}_x86_64']
+                platform_tags = [f"macosx_{min_macosx.replace('.', '_')}_x86_64"]
 
             if include_universal2:
-                platform_tags.append(f'macosx_{min_macosx.replace(".", "_")}_universal2')
+                platform_tags.append(f"macosx_{min_macosx.replace('.', '_')}_universal2")
         else:
             msg = f"Unsupported platform {platform!r}"
             raise Exception(msg)
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+2 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/mypy/blob/e106dd7a0653c24d67597adf3ae6939d7ff9a376/mypy/errors.py#L898'>mypy/errors.py~L898</a>
```diff
                     a.append(" " * DEFAULT_SOURCE_OFFSET + source_line_expanded)
                     marker = "^"
                     if end_line == line and end_column > column:
-                        marker = f'^{"~" * (end_column - column - 1)}'
+                        marker = f"^{'~' * (end_column - column - 1)}"
                     a.append(" " * (DEFAULT_SOURCE_OFFSET + column) + marker)
         return a
 
```
<a href='https://github.com/python/mypy/blob/e106dd7a0653c24d67597adf3ae6939d7ff9a376/mypy/stubtest.py#L1554'>mypy/stubtest.py~L1554</a>
```diff
 
 
 def describe_runtime_callable(signature: inspect.Signature, *, is_async: bool) -> str:
-    return f'{"async " if is_async else ""}def {signature}'
+    return f"{'async ' if is_async else ''}def {signature}"
 
 
 def is_subtype_helper(left: mypy.types.Type, right: mypy.types.Type) -> bool:
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/ca65e087f1f9dfc28e89192b60ce7cfc2e92c674/scripts/stubsabot.py#L533'>scripts/stubsabot.py~L533</a>
```diff
     output = subprocess.check_output(["git", "remote", "get-url", "origin"], text=True).strip()
     match = re.match(r"(git@github.com:|https://github.com/)(?P<owner>[^/]+)/(?P<repo>[^/\s]+)", output)
     assert match is not None, f"Couldn't identify origin's owner: {output!r}"
-    assert match.group("repo").removesuffix(".git") == "typeshed", f'Unexpected repo: {match.group("repo")!r}'
+    assert match.group("repo").removesuffix(".git") == "typeshed", f"Unexpected repo: {match.group('repo')!r}"
     return match.group("owner")
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -3 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/c103ab5e2872ca9496978360802d84c930e50b4e/reflex/custom_components/custom_components.py#L770'>reflex/custom_components/custom_components.py~L770</a>
```diff
     pyproject_toml = _get_package_config()
     project = pyproject_toml["project"]
     console.print(
-        f'Double check the information before publishing: {project["name"]} version {project["version"]}'
+        f"Double check the information before publishing: {project['name']} version {project['version']}"
     )
 
     console.print("Update or enter to keep the current information.")
```
<a href='https://github.com/reflex-dev/reflex/blob/c103ab5e2872ca9496978360802d84c930e50b4e/reflex/custom_components/custom_components.py#L782'>reflex/custom_components/custom_components.py~L782</a>
```diff
     author["name"] = console.ask(f"Author Name", default=author.get("name", ""))
     author["email"] = console.ask(f"Author Email", default=author.get("email", ""))
 
-    console.print(f'Current keywords are: {project.get("keywords") or []}')
+    console.print(f"Current keywords are: {project.get('keywords') or []}")
     keyword_action = console.ask(
         "Keep, replace or append?", choices=["k", "r", "a"], default="k"
     )
```
<a href='https://github.com/reflex-dev/reflex/blob/c103ab5e2872ca9496978360802d84c930e50b4e/reflex/vars/base.py#L364'>reflex/vars/base.py~L364</a>
```diff
 
                 _default_var_type: ClassVar[GenericType] = python_types[0]
 
-            ToVarOperation.__name__ = f'To{cls.__name__.removesuffix("Var")}Operation'
+            ToVarOperation.__name__ = f"To{cls.__name__.removesuffix('Var')}Operation"
 
             _var_subclasses.append(VarSubclassEntry(cls, ToVarOperation, python_types))
 
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+187 -187 lines across 83 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/rest.py#L447'>rotkehlchen/api/rest.py~L447</a>
```diff
             f"{task_str} dies with exception: {greenlet.exception}.\n"
             f"Exception Name: {greenlet.exc_info[0]}\n"
             f"Exception Info: {greenlet.exc_info[1]}\n"
-            f'Traceback:\n {"".join(traceback.format_tb(greenlet.exc_info[2]))}',
+            f"Traceback:\n {''.join(traceback.format_tb(greenlet.exc_info[2]))}",
         )
         # also write an error for the task result if it's not the main greenlet
         if task_id is not None:
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/rest.py#L1479'>rotkehlchen/api/rest.py~L1479</a>
```diff
             return api_response(
                 result=wrap_in_fail_result(
                     f"Failed to add {asset.asset_type!s} {asset.name} "
-                    f'since it already exists. Existing ids: {",".join(identifiers)}'
+                    f"since it already exists. Existing ids: {','.join(identifiers)}"
                 ),
                 status_code=HTTPStatus.CONFLICT,
             )
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/rest.py#L1716'>rotkehlchen/api/rest.py~L1716</a>
```diff
                     message=msg,
                     status_code=HTTPStatus.CONFLICT,
                 )
-            log.debug(f'extracted {len(data["events"])} events from {filepath}')
+            log.debug(f"extracted {len(data['events'])} events from {filepath}")
             self.rotkehlchen.accountant.process_history(
                 start_ts=Timestamp(data["pnl_settings"]["from_timestamp"]),
                 end_ts=Timestamp(data["pnl_settings"]["to_timestamp"]),
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/rest.py#L3269'>rotkehlchen/api/rest.py~L3269</a>
```diff
         """Return the current price of the assets in the target asset currency."""
         log.debug(
             f"Querying the current {target_asset.identifier} price of these assets: "
-            f'{", ".join([asset.identifier for asset in assets])}',
+            f"{', '.join([asset.identifier for asset in assets])}",
         )
         # Type is list instead of tuple here because you can't serialize a tuple
         assets_price: dict[Asset, list[Price | (int | None | bool)]] = {}
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/rest.py#L3376'>rotkehlchen/api/rest.py~L3376</a>
```diff
         """
         log.debug(
             f"Querying the historical {target_asset.identifier} price of these assets: "
-            f'{", ".join(f"{asset.identifier} at {ts}" for asset, ts in assets_timestamp)}',
+            f"{', '.join(f'{asset.identifier} at {ts}' for asset, ts in assets_timestamp)}",
             assets_timestamp=assets_timestamp,
         )
         assets_price: defaultdict[Asset, defaultdict] = defaultdict(
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/fields.py#L533'>rotkehlchen/api/v1/fields.py~L533</a>
```diff
         if self.limit_to is not None and chain_id not in self.limit_to:
             raise ValidationError(
                 f"Given chain_id {value} is not one of "
-                f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
+                f"{','.join([str(x) for x in self.limit_to])} as needed by the endpoint",
             )
 
         return chain_id
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/fields.py#L573'>rotkehlchen/api/v1/fields.py~L573</a>
```diff
         if self.limit_to is not None and chain not in self.limit_to:
             raise ValidationError(
                 f"Given chain {value} is not one of "
-                f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
+                f"{','.join([str(x) for x in self.limit_to])} as needed by the endpoint",
             )
 
         return chain
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/fields.py#L808'>rotkehlchen/api/v1/fields.py~L808</a>
```diff
         if self.limit_to is not None and location not in self.limit_to:
             raise ValidationError(
                 f"Given location {value} is not one of "
-                f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
+                f"{','.join([str(x) for x in self.limit_to])} as needed by the endpoint",
             )
 
         return location
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/fields.py#L930'>rotkehlchen/api/v1/fields.py~L930</a>
```diff
             ):  # noqa: E501
                 raise ValidationError(
                     f"Given file {value.filename} does not end in any of "
-                    f'{",".join(self.allowed_extensions)}',
+                    f"{','.join(self.allowed_extensions)}",
                 )
 
             return value
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/fields.py#L950'>rotkehlchen/api/v1/fields.py~L950</a>
```diff
         ):  # noqa: E501
             raise ValidationError(
                 f"Given file {path} does not end in any of "
-                f'{",".join(self.allowed_extensions)}',
+                f"{','.join(self.allowed_extensions)}",
             )
 
         return path
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L342'>rotkehlchen/api/v1/schemas.py~L342</a>
```diff
             SUPPORTED_CHAIN_IDS
         ):
             raise ValidationError(
-                message=f'rotki does not support evm transactions for {data["evm_chain"]}',
+                message=f"rotki does not support evm transactions for {data['evm_chain']}",
                 field_name="evm_chain",
             )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L482'>rotkehlchen/api/v1/schemas.py~L482</a>
```diff
         ).issubset(valid_ordering_attr):
             error_msg = (
                 f"order_by_attributes for trades can not be "
-                f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
+                f"{','.join(set(data['order_by_attributes']) - valid_ordering_attr)}"
             )
             raise ValidationError(
                 message=error_msg,
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L711'>rotkehlchen/api/v1/schemas.py~L711</a>
```diff
         ).issubset(valid_ordering_attr):
             error_msg = (
                 f"order_by_attributes for history event data can not be "
-                f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
+                f"{','.join(set(data['order_by_attributes']) - valid_ordering_attr)}"
             )
             raise ValidationError(
                 message=error_msg,
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L1001'>rotkehlchen/api/v1/schemas.py~L1001</a>
```diff
         ).issubset(valid_ordering_attr):
             error_msg = (
                 f"order_by_attributes for asset movements can not be "
-                f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
+                f"{','.join(set(data['order_by_attributes']) - valid_ordering_attr)}"
             )
             raise ValidationError(
                 message=error_msg,
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L1195'>rotkehlchen/api/v1/schemas.py~L1195</a>
```diff
 
     if (invalid_oracles := given_set - SETTABLE_CURRENT_PRICE_ORACLES) != set():
         raise ValidationError(
-            f'Invalid current price oracles given: {", ".join([str(x) for x in invalid_oracles])}. ',  # noqa: E501
+            f"Invalid current price oracles given: {', '.join([str(x) for x in invalid_oracles])}. ",  # noqa: E501
         )
 
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L1209'>rotkehlchen/api/v1/schemas.py~L1209</a>
```diff
         oracle_names = [str(oracle) for oracle in historical_price_oracles]
         supported_oracle_names = [str(oracle) for oracle in HistoricalPriceOracle]
         raise ValidationError(
-            f'Invalid historical price oracles in: {", ".join(oracle_names)}. '
-            f'Supported oracles are: {", ".join(supported_oracle_names)}. '
+            f"Invalid historical price oracles in: {', '.join(oracle_names)}. "
+            f"Supported oracles are: {', '.join(supported_oracle_names)}. "
             f"Check there are no repeated ones.",
         )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L1508'>rotkehlchen/api/v1/schemas.py~L1508</a>
```diff
         if data.get("api_key") is None:
             if data["name"] != ExternalService.MONERIUM:
                 raise ValidationError(
-                    message=f'an api key is needed for {data["name"].name.lower()}',
+                    message=f"an api key is needed for {data['name'].name.lower()}",
                     field_name="api_key",
                 )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L1702'>rotkehlchen/api/v1/schemas.py~L1702</a>
```diff
         ).issubset(valid_ordering_attr):
             error_msg = (
                 f"order_by_attributes for accounting report data can not be "
-                f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
+                f"{','.join(set(data['order_by_attributes']) - valid_ordering_attr)}"
             )
             raise ValidationError(
                 message=error_msg,
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L2775'>rotkehlchen/api/v1/schemas.py~L2775</a>
```diff
         ).issubset(valid_ordering_attr):
             error_msg = (
                 f"order_by_attributes for eth2 daily stats can not be "
-                f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
+                f"{','.join(set(data['order_by_attributes']) - valid_ordering_attr)}"
             )
             raise ValidationError(
                 message=error_msg,
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L2888'>rotkehlchen/api/v1/schemas.py~L2888</a>
```diff
                 address = to_checksum_address(data["address"])
             except ValueError as e:
                 raise ValidationError(
-                    f'Given value {data["address"]} is not a valid {data["blockchain"]} address',
+                    f"Given value {data['address']} is not a valid {data['blockchain']} address",
                     field_name="address",
                 ) from e
         else:
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L2929'>rotkehlchen/api/v1/schemas.py~L2929</a>
```diff
             )
         ):
             raise ValidationError(
-                f'Given value {data["address"]} is not a {blockchain} address',
+                f"Given value {data['address']} is not a {blockchain} address",
                 field_name="address",
             )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L3096'>rotkehlchen/api/v1/schemas.py~L3096</a>
```diff
             == data["location_data_snapshot"][0].time
         ):  # noqa: E501
             raise ValidationError(
-                f'timestamp provided {data["timestamp"]} is not the same as the '
+                f"timestamp provided {data['timestamp']} is not the same as the "
                 f"one for the entries provided.",
             )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L3357'>rotkehlchen/api/v1/schemas.py~L3357</a>
```diff
             ).fetchone()[0]
             if tx_count > 0:
                 raise ValidationError(
-                    message=f'tx_hash {data["tx_hash"].hex()} for {data["evm_chain"]} already present in the database',  # noqa: E501
+                    message=f"tx_hash {data['tx_hash'].hex()} for {data['evm_chain']} already present in the database",  # noqa: E501
                     field_name="tx_hash",
                 )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/api/v1/schemas.py#L3367'>rotkehlchen/api/v1/schemas.py~L3367</a>
```diff
             ).fetchone()[0]
             if accounts_count == 0:
                 raise ValidationError(
-                    message=f'address {data["associated_address"]} provided is not tracked by rotki for {data["evm_chain"]}',  # noqa: E501
+                    message=f"address {data['associated_address']} provided is not tracked by rotki for {data['evm_chain']}",  # noqa: E501
                     field_name="associated_address",
                 )
 
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/chain/aggregator.py#L535'>rotkehlchen/chain/aggregator.py~L535</a>
```diff
         if len(bad_accounts) != 0:
             word = "already" if append_or_remove == "append" else "don't"
             raise InputError(
-                f'Blockchain account/s {",".join(bad_accounts)} {word} exist',
+                f"Blockchain account/s {','.join(bad_accounts)} {word} exist",
             )
 
     @protect_with_lock(arguments_matter=True)
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/chain/aggregator.py#L781'>rotkehlchen/chain/aggregator.py~L781</a>
```diff
         if len(unknown_accounts) != 0:
             raise InputError(
                 f"Tried to remove unknown {blockchain.value} "
-                f'accounts {",".join(unknown_accounts)}',
+                f"accounts {','.join(unknown_accounts)}",
             )
 
         self.modify_blockchain_accounts(
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/chain/ethereum/airdrops.py#L41'>rotkehlchen/chain/ethereum/airdrops.py~L41</a>
```diff
 log = RotkehlchenLogsAdapter(logger)
 
 AIRDROPS_REPO_BASE: Final = (
-    f'https://raw.githubusercontent.com/rotki/data/{"main" if is_production() else "develop"}'  # noqa: E501
+    f"https://raw.githubusercontent.com/rotki/data/{'main' if is_production() else 'develop'}"  # noqa: E501
 )
 AIRDROPS_INDEX: Final = f"{AIRDROPS_REPO_BASE}/airdrops/index_v3.json"
 ETAG_CACHE_KEY: Final = "ETag"
```
<a href='https://github.com/rotki/rotki/blob/20fb816f2d05bbf585e7ad98cb6047fa69b65874/rotkehlchen/chain/ethereum/modules/compound/v3/compound.py#L75'>rotkehlchen/chain/ethereum/modules/compound/v3/compound.py~L75</a>
```diff
                 rates_calls[balance_type].append((
                     token_address,
                     token_contract.encode(
-                        method_name=f'get{"Supply" if balance_type == BalanceType.ASSET else "Borrow"}Rate',  # noqa: E501
+                        method_name=f"get{'Supply' if balance_type == BalanceType.ASSET else 'Borrow'}Rate",  ...*[Comment body truncated]*

---

_Comment by @MichaReiser on 2024-10-22 08:40_

This not only fixes the issue where Ruff failed to change the quotes in Py312+ but now also ensures that f-strings use the preferred quote styles if nested literals uses the other quotes

---

_Renamed from "Alternate quotes for strings inside f-strings when targeting Py312+ and preview mode is on" to "Alternate quotes for strings inside f-strings in preview " by @MichaReiser on 2024-10-22 09:09_

---

_Marked ready for review by @MichaReiser on 2024-10-22 10:03_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-22 10:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:139 on 2024-10-22 10:26_

This might make `rustfmt` mad so might want to do this locally

```suggestion
                if fstring
                    .elements
                    .expressions()
                    .any(|expression| {
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:578 on 2024-10-22 11:06_

It's interesting that as a side effect we'll start fixing certain code that would raise a syntax error pre-3.12.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:729 on 2024-10-22 11:11_

Is this correct? The outer single quotes got formatted to use double quotes even though the expression has a double quoted string, thus not using alternate quote style.

---

_@dhruvmanila reviewed on 2024-10-22 11:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:139 on 2024-10-22 11:31_

Oh it's on elements. I always try to call it on the fstring

---

_@MichaReiser reviewed on 2024-10-22 11:31_

---

_@MichaReiser reviewed on 2024-10-22 11:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:729 on 2024-10-22 11:32_

Yes. The idea is that we use the same quote selection criteria as everywhere else and we keep using double quotes to avoid unnecessary escaping the single quote 

---

_@dhruvmanila approved on 2024-10-22 12:04_

This is great! I really like the ecosystem changes as well. I played around with random f-string with quotes but couldn't really find anything unexpected.

---

_Comment by @MichaReiser on 2024-10-22 16:09_

I'll do another review of my own changes tomorrow and then plan to land this.

---

_Merged by @MichaReiser on 2024-10-23 05:57_

---

_Closed by @MichaReiser on 2024-10-23 05:57_

---

_Branch deleted on 2024-10-23 05:57_

---
