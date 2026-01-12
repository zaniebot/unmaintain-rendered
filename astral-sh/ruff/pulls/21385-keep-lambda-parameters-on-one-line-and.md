```yaml
number: 21385
title: Keep lambda parameters on one line and parenthesize the body if it expands
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: brent/indent-lambda-params
created_at: 2025-11-11T15:57:34Z
updated_at: 2025-12-12T17:02:28Z
url: https://github.com/astral-sh/ruff/pull/21385
synced_at: 2026-01-12T15:57:22Z
```

# Keep lambda parameters on one line and parenthesize the body if it expands

---

_@ntBre_

## Summary

This PR makes two changes to our formatting of `lambda` expressions:
1. We now parenthesize the body expression if it expands
2. We now try to keep the parameters on a single line

The latter of these fixes #8179:

Black formatting and this PR's formatting:

```py
def a():
    return b(
        c,
        d,
        e,
        f=lambda self, *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
            *args, **kwargs
        ),
    )
```

Stable Ruff formatting

```py
def a():
    return b(
        c,
        d,
        e,
        f=lambda self,
        *args,
        **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs),
    )
```

We don't parenthesize the body expression here because the call to `aaaa...` has its own parentheses, but adding a binary operator shows the new parenthesization:

```diff
@@ -3,7 +3,7 @@
         c,
         d,
         e,
-        f=lambda self, *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
-            *args, **kwargs
-        ) + 1,
+        f=lambda self, *args, **kwargs: (
+            aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
+        ),
     )
```

This is actually a new divergence from Black, which formats this input like this:

```py
def a():
    return b(
        c,
        d,
        e,
        f=lambda self, *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
            *args, **kwargs
        )
        + 1,
    )
```

But I think this is an improvement, unlike the case from #8179.

One other, smaller benefit is that because we now add parentheses to lambda bodies, we also remove redundant parentheses:

```diff
 @pytest.mark.parametrize(
     "f",
     [
-        lambda x: (x.expanding(min_periods=5).cov(x, pairwise=True)),
-        lambda x: (x.expanding(min_periods=5).corr(x, pairwise=True)),
+        lambda x: x.expanding(min_periods=5).cov(x, pairwise=True),
+        lambda x: x.expanding(min_periods=5).corr(x, pairwise=True),
     ],
 )
 def test_moment_functions_zero_length_pairwise(f):
```

## Test Plan

New tests taken from #8465 and probably a few more I should grab from the ecosystem results.


---

_Label `formatter` added by @ntBre on 2025-11-11 15:57_

---

_Label `preview` added by @ntBre on 2025-11-11 15:57_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 16:11_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+900 -814 lines in 68 files in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+6 -6 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/extractors/crf_entity_extractor.py#L101'>rasa/nlu/extractors/crf_entity_extractor.py~L101</a>
```diff
         CRFEntityExtractorOptions.SUFFIX1: lambda crf_token: crf_token.text[-1:],
         CRFEntityExtractorOptions.BIAS: lambda _: "bias",
         CRFEntityExtractorOptions.POS: lambda crf_token: crf_token.pos_tag,
-        CRFEntityExtractorOptions.POS2: lambda crf_token: crf_token.pos_tag[:2]
-        if crf_token.pos_tag is not None
-        else None,
+        CRFEntityExtractorOptions.POS2: lambda crf_token: (
+            crf_token.pos_tag[:2] if crf_token.pos_tag is not None else None
+        ),
         CRFEntityExtractorOptions.UPPER: lambda crf_token: crf_token.text.isupper(),
         CRFEntityExtractorOptions.DIGIT: lambda crf_token: crf_token.text.isdigit(),
         CRFEntityExtractorOptions.PATTERN: lambda crf_token: crf_token.pattern,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L86'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L86</a>
```diff
         "suffix2": lambda token: token.text[-2:],
         "suffix1": lambda token: token.text[-1:],
         "pos": lambda token: token.data.get(POS_TAG_KEY, None),
-        "pos2": lambda token: token.data.get(POS_TAG_KEY, [])[:2]
-        if POS_TAG_KEY in token.data
-        else None,
+        "pos2": lambda token: (
+            token.data.get(POS_TAG_KEY, [])[:2] if POS_TAG_KEY in token.data else None
+        ),
         "upper": lambda token: token.text.isupper(),
         "digit": lambda token: token.text.isdigit(),
     }
```

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PlasmaPy/PlasmaPy/blob/f95c6a4b6629f094b39837614f50741dc13e2282/src/plasmapy/particles/atomic.py#L1245'>src/plasmapy/particles/atomic.py~L1245</a>
```diff
         # If it has been indicated that the user wants the interpolator, construct
         # an anonymous function to handle units and sanitize IO
         if return_interpolator:
-            return (
-                lambda x: np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
+            return lambda x: (
+                np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
             )
 
         return (
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+10 -13 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/8256ee16d1de5b47ad996af82608d2bb836dfe1d/airflow-core/tests/unit/cli/commands/test_config_command.py#L355'>airflow-core/tests/unit/cli/commands/test_config_command.py~L355</a>
```diff
     def test_lint_detects_multiple_issues(self, stdout_capture):
         with mock.patch(
             "airflow.configuration.conf.has_option",
-            side_effect=lambda section, option, lookup_from_deprecated: option
-            in ["check_slas", "strict_dataset_uri_validation"],
+            side_effect=lambda section, option, lookup_from_deprecated: (
+                option in ["check_slas", "strict_dataset_uri_validation"]
+            ),
         ):
             with stdout_capture as temp_stdout:
                 config_command.lint_config(cli_parser.get_parser().parse_args(["config", "lint"]))
```
<a href='https://github.com/apache/airflow/blob/8256ee16d1de5b47ad996af82608d2bb836dfe1d/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/executors/test_kubernetes_executor.py#L992'>providers/cncf/kubernetes/tests/unit/cncf/kubernetes/executors/test_kubernetes_executor.py~L992</a>
```diff
         mock_ti.queued_by_job_id = "10"  # scheduler_job would have updated this after the first adoption
         executor.scheduler_job_id = "20"
         # assume success adopting, `adopt_launched_task` pops `ti_key` from `tis_to_flush_by_key`
-        mock_adopt_launched_task.side_effect = (
-            lambda client, pod, tis_to_flush_by_key: tis_to_flush_by_key.pop(ti_key)
+        mock_adopt_launched_task.side_effect = lambda client, pod, tis_to_flush_by_key: (
+            tis_to_flush_by_key.pop(ti_key)
         )
 
         reset_tis = executor.try_adopt_task_instances([mock_ti])
```
<a href='https://github.com/apache/airflow/blob/8256ee16d1de5b47ad996af82608d2bb836dfe1d/providers/docker/tests/unit/docker/operators/test_docker.py#L153'>providers/docker/tests/unit/docker/operators/test_docker.py~L153</a>
```diff
         self.client_mock.attach.return_value = self.log_messages
 
         # If logs() is called with tail then only return the last value, otherwise return the whole log.
-        self.client_mock.logs.side_effect = (
-            lambda **kwargs: iter(self.log_messages[-kwargs["tail"] :])
-            if "tail" in kwargs
-            else iter(self.log_messages)
+        self.client_mock.logs.side_effect = lambda **kwargs: (
+            iter(self.log_messages[-kwargs["tail"] :]) if "tail" in kwargs else iter(self.log_messages)
         )
 
         docker_api_client_patcher.return_value = self.client_mock
```
<a href='https://github.com/apache/airflow/blob/8256ee16d1de5b47ad996af82608d2bb836dfe1d/providers/docker/tests/unit/docker/operators/test_docker.py#L622'>providers/docker/tests/unit/docker/operators/test_docker.py~L622</a>
```diff
         self.client_mock.pull.return_value = [b'{"status":"pull log"}']
         self.client_mock.attach.return_value = iter([b"container log 1 \n", b"container log 2\n"])
         # Make sure the logs side effect is updated after the change
-        self.client_mock.attach.side_effect = (
-            lambda **kwargs: iter(self.log_messages[-kwargs["tail"] :])
-            if "tail" in kwargs
-            else iter(self.log_messages)
+        self.client_mock.attach.side_effect = lambda **kwargs: (
+            iter(self.log_messages[-kwargs["tail"] :]) if "tail" in kwargs else iter(self.log_messages)
         )
 
         kwargs = {
```
<a href='https://github.com/apache/airflow/blob/8256ee16d1de5b47ad996af82608d2bb836dfe1d/providers/http/tests/unit/http/sensors/test_http.py#L302'>providers/http/tests/unit/http/sensors/test_http.py~L302</a>
```diff
             method="GET",
             endpoint="/search",
             data={"client": "ubuntu", "q": "airflow"},
-            response_check=lambda response: ("apache/airflow" in response.text),
+            response_check=lambda response: "apache/airflow" in response.text,
             headers={},
         )
         op.execute({})
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+34 -25 lines across 7 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/superset/tags/api.py#L598'>superset/tags/api.py~L598</a>
```diff
     @statsd_metrics
     @rison({"type": "array", "items": {"type": "integer"}})
     @event_logger.log_this_with_context(
-        action=lambda self, *args, **kwargs: f"{self.__class__.__name__}"
-        f".favorite_status",
+        action=lambda self, *args, **kwargs: (
+            f"{self.__class__.__name__}.favorite_status"
+        ),
         log_to_statsd=False,
     )
     def favorite_status(self, **kwargs: Any) -> Response:
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/superset/tags/api.py#L696'>superset/tags/api.py~L696</a>
```diff
     @safe
     @statsd_metrics
     @event_logger.log_this_with_context(
-        action=lambda self, *args, **kwargs: f"{self.__class__.__name__}"
-        f".remove_favorite",
+        action=lambda self, *args, **kwargs: (
+            f"{self.__class__.__name__}.remove_favorite"
+        ),
         log_to_statsd=False,
     )
     def remove_favorite(self, pk: int) -> Response:
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/datasource_tests.py#L213'>tests/integration_tests/datasource_tests.py~L213</a>
```diff
     def test_external_metadata_by_name_for_virtual_table_uses_mutator(self):
         self.login(ADMIN_USERNAME)
         with create_and_cleanup_table() as tbl:
-            current_app.config["SQL_QUERY_MUTATOR"] = (
-                lambda sql, **kwargs: "SELECT 456 as intcol, 'def' as mutated_strcol"
+            current_app.config["SQL_QUERY_MUTATOR"] = lambda sql, **kwargs: (
+                "SELECT 456 as intcol, 'def' as mutated_strcol"
             )
 
             params = prison.dumps({
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/datasource_tests.py#L339'>tests/integration_tests/datasource_tests.py~L339</a>
```diff
 
         pytest.raises(
             SupersetGenericDBErrorException,
-            lambda: db.session.query(SqlaTable)
-            .filter_by(id=tbl.id)
-            .one_or_none()
-            .external_metadata(),
+            lambda: (
+                db.session.query(SqlaTable)
+                .filter_by(id=tbl.id)
+                .one_or_none()
+                .external_metadata()
+            ),
         )
 
         resp = self.client.get(url)
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/db_engine_specs/presto_tests.py#L81'>tests/integration_tests/db_engine_specs/presto_tests.py~L81</a>
```diff
     def verify_presto_column(self, column, expected_results):
         inspector = mock.Mock()
         preparer = inspector.engine.dialect.identifier_preparer
-        preparer.quote_identifier = preparer.quote = preparer.quote_schema = (
-            lambda x: f'"{x}"'
+        preparer.quote_identifier = preparer.quote = preparer.quote_schema = lambda x: (
+            f'"{x}"'
         )
         row = mock.Mock()
         row.Column, row.Type, row.Null = column
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/db_engine_specs/presto_tests.py#L827'>tests/integration_tests/db_engine_specs/presto_tests.py~L827</a>
```diff
     def test_show_columns(self):
         inspector = mock.MagicMock()
         preparer = inspector.engine.dialect.identifier_preparer
-        preparer.quote_identifier = preparer.quote = preparer.quote_schema = (
-            lambda x: f'"{x}"'
+        preparer.quote_identifier = preparer.quote = preparer.quote_schema = lambda x: (
+            f'"{x}"'
         )
         inspector.bind.execute.return_value.fetchall = mock.MagicMock(
             return_value=["a", "b"]
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/db_engine_specs/presto_tests.py#L843'>tests/integration_tests/db_engine_specs/presto_tests.py~L843</a>
```diff
     def test_show_columns_with_schema(self):
         inspector = mock.MagicMock()
         preparer = inspector.engine.dialect.identifier_preparer
-        preparer.quote_identifier = preparer.quote = preparer.quote_schema = (
-            lambda x: f'"{x}"'
+        preparer.quote_identifier = preparer.quote = preparer.quote_schema = lambda x: (
+            f'"{x}"'
         )
         inspector.bind.execute.return_value.fetchall = mock.MagicMock(
             return_value=["a", "b"]
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/security/api_tests.py#L187'>tests/integration_tests/security/api_tests.py~L187</a>
```diff
         self.assert500(self._get_guest_token_with_rls(rls_rule))
 
     @with_config({
-        "GUEST_TOKEN_VALIDATOR_HOOK": lambda x: len(x["rls"]) == 1
-        and "tenant_id=" in x["rls"][0]["clause"]
+        "GUEST_TOKEN_VALIDATOR_HOOK": lambda x: (
+            len(x["rls"]) == 1 and "tenant_id=" in x["rls"][0]["clause"]
+        )
     })
     def test_guest_validator_hook_real_world_example_positive(self):
         """
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/integration_tests/security/api_tests.py#L201'>tests/integration_tests/security/api_tests.py~L201</a>
```diff
         self.assert200(self._get_guest_token_with_rls(rls_rule))
 
     @with_config({
-        "GUEST_TOKEN_VALIDATOR_HOOK": lambda x: len(x["rls"]) == 1
-        and "tenant_id=" in x["rls"][0]["clause"]
+        "GUEST_TOKEN_VALIDATOR_HOOK": lambda x: (
+            len(x["rls"]) == 1 and "tenant_id=" in x["rls"][0]["clause"]
+        )
     })
     def test_guest_validator_hook_real_world_example_negative(self):
         """
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/unit_tests/datasets/test_datetime_format_detector.py#L44'>tests/unit_tests/datasets/test_datetime_format_detector.py~L44</a>
```diff
     dataset.database.get_sqla_engine.return_value.__exit__.return_value = None
 
     # Mock apply_limit_to_sql to return SQL with LIMIT
-    dataset.database.apply_limit_to_sql = (
-        lambda sql, limit, force: f"{sql} LIMIT {limit}"
+    dataset.database.apply_limit_to_sql = lambda sql, limit, force: (
+        f"{sql} LIMIT {limit}"
     )
 
     return dataset
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/unit_tests/importexport/api_test.py#L48'>tests/unit_tests/importexport/api_test.py~L48</a>
```diff
     mocked_export_result = [
         (
             "metadata.yaml",
-            lambda: "version: 1.0.0\ntype: assets\ntimestamp: '2022-01-01T00:00:00+00:00'\n",  # noqa: E501
+            lambda: (
+                "version: 1.0.0\ntype: assets\ntimestamp: '2022-01-01T00:00:00+00:00'\n"
+            ),  # noqa: E501
         ),
         ("databases/example.yaml", lambda: "<DATABASE CONTENTS>"),
     ]
```
<a href='https://github.com/apache/superset/blob/e5b7e38a305437b81526f542f750d152dd6870ae/tests/unit_tests/utils/test_core.py#L635'>tests/unit_tests/utils/test_core.py~L635</a>
```diff
 
 
 @with_config({
-    "USER_AGENT_FUNC": lambda database,
-    source: f"{database.database_name} {source.name}"
+    "USER_AGENT_FUNC": lambda database, source: (
+        f"{database.database_name} {source.name}"
+    )
 })
 def test_get_user_agent_custom(mocker: MockerFixture, app_context: None) -> None:
     database_mock = mocker.MagicMock()
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+15 -12 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/a367657230ac67dc46de62f7d71e4e5a75e124ff/samcli/lib/cli_validation/image_repository_validation.py#L70'>samcli/lib/cli_validation/image_repository_validation.py~L70</a>
```diff
 
             validators = [
                 Validator(
-                    validation_function=lambda: bool(image_repository)
-                    + bool(image_repositories)
-                    + bool(resolve_image_repos)
-                    > 1,
+                    validation_function=lambda: (
+                        bool(image_repository) + bool(image_repositories) + bool(resolve_image_repos) > 1
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/a367657230ac67dc46de62f7d71e4e5a75e124ff/samcli/lib/cli_validation/image_repository_validation.py#L82'>samcli/lib/cli_validation/image_repository_validation.py~L82</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and not (image_repository or image_repositories or resolve_image_repos)
-                    and required,
+                    validation_function=lambda: (
+                        not guided and not (image_repository or image_repositories or resolve_image_repos) and required
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/a367657230ac67dc46de62f7d71e4e5a75e124ff/samcli/lib/cli_validation/image_repository_validation.py#L92'>samcli/lib/cli_validation/image_repository_validation.py~L92</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and (
-                        image_repositories
-                        and not resolve_image_repos
-                        and not _is_all_image_funcs_provided(template_file, image_repositories, parameters_overrides)
+                    validation_function=lambda: (
+                        not guided
+                        and (
+                            image_repositories
+                            and not resolve_image_repos
+                            and not _is_all_image_funcs_provided(
+                                template_file, image_repositories, parameters_overrides
+                            )
+                        )
                     ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories", ctx=ctx, message=image_repos_error_msg
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+30 -26 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L83'>crazy_functions/agent_fns/general.py~L83</a>
```diff
             }
             kwargs.update(agent_kwargs)
             agent_handle = agent_cls(**kwargs)
-            agent_handle._print_received_message = (
-                lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+            agent_handle._print_received_message = lambda a, b: (
+                self.gpt_academic_print_override(agent_kwargs, a, b)
             )
             for d in agent_handle._reply_func_list:
                 if (
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L93'>crazy_functions/agent_fns/general.py~L93</a>
```diff
                 ):
                     d["reply_func"] = gpt_academic_generate_oai_reply
             if agent_kwargs["name"] == "user_proxy":
-                agent_handle.get_human_input = (
-                    lambda a: self.gpt_academic_get_human_input(user_proxy, a)
+                agent_handle.get_human_input = lambda a: (
+                    self.gpt_academic_get_human_input(user_proxy, a)
                 )
                 user_proxy = agent_handle
             if agent_kwargs["name"] == "assistant":
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L134'>crazy_functions/agent_fns/general.py~L134</a>
```diff
                 kwargs = {"code_execution_config": code_execution_config}
                 kwargs.update(agent_kwargs)
                 agent_handle = agent_cls(**kwargs)
-                agent_handle._print_received_message = (
-                    lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+                agent_handle._print_received_message = lambda a, b: (
+                    self.gpt_academic_print_override(agent_kwargs, a, b)
                 )
                 agents_instances.append(agent_handle)
                 if agent_kwargs["name"] == "user_proxy":
                     user_proxy = agent_handle
-                    user_proxy.get_human_input = (
-                        lambda a: self.gpt_academic_get_human_input(user_proxy, a)
+                    user_proxy.get_human_input = lambda a: (
+                        self.gpt_academic_get_human_input(user_proxy, a)
                     )
             try:
                 groupchat = autogen.GroupChat(
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L150'>crazy_functions/agent_fns/general.py~L150</a>
```diff
                 manager = autogen.GroupChatManager(
                     groupchat=groupchat, **self.define_group_chat_manager_config()
                 )
-                manager._print_received_message = (
-                    lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+                manager._print_received_message = lambda a, b: (
+                    self.gpt_academic_print_override(agent_kwargs, a, b)
                 )
                 manager.get_human_input = lambda a: self.gpt_academic_get_human_input(
                     manager, a
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/crazy_utils.py#L299'>crazy_functions/crazy_utils.py~L299</a>
```diff
         retry_op = retry_times_at_unknown_error
         exceeded_cnt = 0
         mutable[index][2] = "执行中"
-        detect_timeout = (
-            lambda: len(mutable[index]) >= 2
+        detect_timeout = lambda: (
+            len(mutable[index]) >= 2
             and (time.time() - mutable[index][1]) > watch_dog_patience
         )
         while True:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/paper_processor/paper_llm_ranker.py#L143'>crazy_functions/review_fns/paper_processor/paper_llm_ranker.py~L143</a>
```diff
                     )
                 elif search_criteria.query_type == "review":
                     papers.sort(
-                        key=lambda x: 1
-                        if any(
-                            keyword in (getattr(x, "title", "") or "").lower()
-                            or keyword in (getattr(x, "abstract", "") or "").lower()
-                            for keyword in ["review", "survey", "overview"]
-                        )
-                        else 0,
+                        key=lambda x: (
+                            1
+                            if any(
+                                keyword in (getattr(x, "title", "") or "").lower()
+                                or keyword in (getattr(x, "abstract", "") or "").lower()
+                                for keyword in ["review", "survey", "overview"]
+                            )
+                            else 0
+                        ),
                         reverse=True,
                     )
             return papers[:top_k]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/paper_processor/paper_llm_ranker.py#L164'>crazy_functions/review_fns/paper_processor/paper_llm_ranker.py~L164</a>
```diff
         if search_criteria and search_criteria.query_type == "review":
             papers = sorted(
                 papers,
-                key=lambda x: 1
-                if any(
-                    keyword in (getattr(x, "title", "") or "").lower()
-                    or keyword in (getattr(x, "abstract", "") or "").lower()
-                    for keyword in ["review", "survey", "overview"]
-                )
-                else 0,
+                key=lambda x: (
+                    1
+                    if any(
+                        keyword in (getattr(x, "title", "") or "").lower()
+                        or keyword in (getattr(x, "abstract", "") or "").lower()
+                        for keyword in ["review", "survey", "overview"]
+                    )
+                    else 0
+                ),
                 reverse=True,
             )
 
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+135 -119 lines across 7 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/datafusion/__init__.py#L246'>ibis/backends/datafusion/__init__.py~L246</a>
```diff
 
         for name, func in inspect.getmembers(
             udfs,
-            predicate=lambda m: callable(m)
-            and not m.__name__.startswith("_")
-            and m.__module__ == udfs.__name__,
+            predicate=lambda m: (
+                callable(m)
+                and not m.__name__.startswith("_")
+                and m.__module__ == udfs.__name__
+            ),
         ):
             annotations = typing.get_type_hints(func)
             argnames = list(inspect.signature(func).parameters.keys())
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/sql/dialects.py#L241'>ibis/backends/sql/dialects.py~L241</a>
```diff
             sge.ArrayAgg: rename_func("array_agg"),
             sge.ArraySort: rename_func("array_sort"),
             sge.Length: rename_func("char_length"),
-            sge.TryCast: lambda self,
-            e: f"TRY_CAST({e.this.sql(self.dialect)} AS {e.to.sql(self.dialect)})",
+            sge.TryCast: lambda self, e: (
+                f"TRY_CAST({e.this.sql(self.dialect)} AS {e.to.sql(self.dialect)})"
+            ),
             sge.DayOfYear: rename_func("dayofyear"),
             sge.DayOfWeek: rename_func("dayofweek"),
             sge.DayOfMonth: rename_func("dayofmonth"),
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_aggregation.py#L1278'>ibis/backends/tests/test_aggregation.py~L1278</a>
```diff
         )
         .groupby("bigint_col")
         .string_col.agg(
-            lambda s: (np.nan if pd.isna(s).all() else pandas_sep.join(s.values))
+            lambda s: np.nan if pd.isna(s).all() else pandas_sep.join(s.values)
         )
         .rename("tmp")
         .sort_index()
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L214'>ibis/backends/tests/test_window.py~L214</a>
```diff
         ),
         param(
             lambda t, win: t.double_col.cummean().over(win),
-            lambda t: (t.double_col.expanding().mean().reset_index(drop=True, level=0)),
+            lambda t: t.double_col.expanding().mean().reset_index(drop=True, level=0),
             id="cummean",
             marks=pytest.mark.notimpl(["druid"], raises=PyDruidProgrammingError),
         ),
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L279'>ibis/backends/tests/test_window.py~L279</a>
```diff
         ),
         param(
             lambda t, win: t.double_col.mean().over(win),
-            lambda gb: (
-                gb.double_col.expanding().mean().reset_index(drop=True, level=0)
-            ),
+            lambda gb: gb.double_col.expanding().mean().reset_index(drop=True, level=0),
             id="mean",
             marks=pytest.mark.notimpl(["druid"], raises=PyDruidProgrammingError),
         ),
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L335'>ibis/backends/tests/test_window.py~L335</a>
```diff
     [
         param(
             lambda t, win: t.double_col.mean().over(win),
-            lambda df: (df.double_col.expanding().mean()),
+            lambda df: df.double_col.expanding().mean(),
             id="mean",
             marks=[
                 pytest.mark.notimpl(
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L350'>ibis/backends/tests/test_window.py~L350</a>
```diff
             # Disabled on PySpark and Spark backends because in pyspark<3.0.0,
             # Pandas UDFs are only supported on unbounded windows
             lambda t, win: mean_udf(t.double_col).over(win),
-            lambda df: (df.double_col.expanding().mean()),
+            lambda df: df.double_col.expanding().mean(),
             id="mean_udf",
             marks=[
                 pytest.mark.notimpl(
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L538'>ibis/backends/tests/test_window.py~L538</a>
```diff
     [
         param(
             lambda t, win: t.double_col.mean().over(win),
-            lambda gb: (gb.double_col.transform("mean")),
+            lambda gb: gb.double_col.transform("mean"),
             id="mean",
             marks=pytest.mark.notimpl(["druid"], raises=PyDruidProgrammingError),
         ),
         param(
             lambda t, win: mean_udf(t.double_col).over(win),
-            lambda gb: (gb.double_col.transform("mean")),
+            lambda gb: gb.double_col.transform("mean"),
             id="mean_udf",
             marks=[
                 pytest.mark.notimpl(
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/test_window.py#L1192'>ibis/backends/tests/test_window.py~L1192</a>
```diff
     expected = (
         df.sort_values("int_col")
         .groupby(df["int_col"].notnull())
-        .apply(lambda df: (df.int_col.rank(method="min").sub(1).div(len(df) - 1)))
+        .apply(lambda df: df.int_col.rank(method="min").sub(1).div(len(df) - 1))
         .T.reset_index(drop=True)
         .iloc[:, 0]
         .rename(expr.get_name())
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L35'>ibis/backends/tests/tpc/ds/test_queries.py~L35</a>
```diff
         )
         .join(customer, _.ctr_customer_sk == customer.c_customer_sk)
         .filter(
-            lambda t: t.ctr_total_return
-            > ctr2.filter(t.ctr_store_sk == ctr2.ctr_store_sk)
-            .ctr_total_return.mean()
-            .as_scalar()
-            * 1.2
+            lambda t: (
+                t.ctr_total_return
+                > ctr2.filter(t.ctr_store_sk == ctr2.ctr_store_sk)
+                .ctr_total_return.mean()
+                .as_scalar()
+                * 1.2
+            )
         )
         .select(_.c_customer_id)
         .order_by(_.c_customer_id)
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L783'>ibis/backends/tests/tpc/ds/test_queries.py~L783</a>
```diff
                 > 0
             ),
             lambda t: (
-                web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
-                .filter(
-                    t.c_customer_sk == web_sales.ws_bill_customer_sk,
-                    _.d_year == 2002,
-                    _.d_moy.between(1, 1 + 3),
+                (
+                    web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
+                    .filter(
+                        t.c_customer_sk == web_sales.ws_bill_customer_sk,
+                        _.d_year == 2002,
+                        _.d_moy.between(1, 1 + 3),
+                    )
+                    .count()
+                    > 0
                 )
-                .count()
-                > 0
-            )
-            | (
-                catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
-                .filter(
-                    t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
-                    _.d_year == 2002,
-                    _.d_moy.between(1, 1 + 3),
+                | (
+                    catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
+                    .filter(
+                        t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
+                        _.d_year == 2002,
+                        _.d_moy.between(1, 1 + 3),
+                    )
+                    .count()
+                    > 0
                 )
-                .count()
-                > 0
             ),
         )
         .group_by(
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L1037'>ibis/backends/tests/tpc/ds/test_queries.py~L1037</a>
```diff
             _.d_date.between(date("2002-02-01"), date("2002-04-02")),
             _.ca_state == "GA",
             _.cc_county == "Williamson County",
-            lambda t: catalog_sales.filter(
-                t.cs_order_number == _.cs_order_number,
-                t.cs_warehouse_sk != _.cs_warehouse_sk,
-            ).count()
-            > 0,
-            lambda t: catalog_returns.filter(
-                t.cs_order_number == _.cr_order_number
-            ).count()
-            == 0,
+            lambda t: (
+                catalog_sales.filter(
+                    t.cs_order_number == _.cs_order_number,
+                    t.cs_warehouse_sk != _.cs_warehouse_sk,
+                ).count()
+                > 0
+            ),
+            lambda t: (
+                catalog_returns.filter(t.cs_order_number == _.cr_order_number).count()
+                == 0
+            ),
         )
         .agg(**{
             "order count": _.cs_order_number.nunique(),
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L2057'>ibis/backends/tests/tpc/ds/test_queries.py~L2057</a>
```diff
         item.view()
         .filter(
             _.i_manufact_id.between(738, 738 + 40),
-            lambda i1: item.filter(
-                lambda s: (
-                    (i1.i_manufact == s.i_manufact)
-                    & (
-                        (
-                            (s.i_category == "Women")
-                            & s.i_color.isin(("powder", "khaki"))
-                            & s.i_units.isin(("Ounce", "Oz"))
-                            & s.i_size.isin(("medium", "extra large"))
-                        )
-                        | (
-                            (s.i_category == "Women")
-                            & s.i_color.isin(("brown", "honeydew"))
-                            & s.i_units.isin(("Bunch", "Ton"))
-                            & s.i_size.isin(("N/A", "small"))
-                        )
-                        | (
-                            (s.i_category == "Men")
-                            & s.i_color.isin(("floral", "deep"))
-                            & s.i_units.isin(("N/A", "Dozen"))
-                            & s.i_size.isin(("petite", "petite"))
-                        )
-                        | (
-                            (s.i_category == "Men")
-                            & s.i_color.isin(("light", "cornflower"))
-                            & s.i_units.isin(("Box", "Pound"))
-                            & s.i_size.isin(("medium", "extra large"))
-                        )
-                    )
-                )
-                | (
-                    (i1.i_manufact == s.i_manufact)
-                    & (
+            lambda i1: (
+                item.filter(
+                    lambda s: (
                         (
-                            (s.i_category == "Women")
-                            & s.i_color.isin(("midnight", "snow"))
-                            & s.i_units.isin(("Pallet", "Gross"))
-                            & s.i_size.isin(("medium", "extra large"))
-                        )
-                        | (
-                            (s.i_category == "Women")
-                            & s.i_color.isin(("cyan", "papaya"))
-                            & s.i_units.isin(("Cup", "Dram"))
-                            & s.i_size.isin(("N/A", "small"))
-                        )
-                        | (
-                            (s.i_category == "Men")
-                            & s.i_color.isin(("orange", "frosted"))
-                            & s.i_units.isin(("Each", "Tbl"))
-                            & s.i_size.isin(("petite", "petite"))
+                            (i1.i_manufact == s.i_manufact)
+                            & (
+                                (
+                                    (s.i_category == "Women")
+                                    & s.i_color.isin(("powder", "khaki"))
+                                    & s.i_units.isin(("Ounce", "Oz"))
+                                    & s.i_size.isin(("medium", "extra large"))
+                                )
+                                | (
+                                    (s.i_category == "Women")
+                                    & s.i_color.isin(("brown", "honeydew"))
+                                    & s.i_units.isin(("Bunch", "Ton"))
+                                    & s.i_size.isin(("N/A", "small"))
+                                )
+                                | (
+                                    (s.i_category == "Men")
+                                    & s.i_color.isin(("floral", "deep"))
+                                    & s.i_units.isin(("N/A", "Dozen"))
+                                    & s.i_size.isin(("petite", "petite"))
+                                )
+                                | (
+                                    (s.i_category == "Men")
+                                    & s.i_color.isin(("light", "cornflower"))
+                                    & s.i_units.isin(("Box", "Pound"))
+                                    & s.i_size.isin(("medium", "extra large"))
+                                )
+                            )
                         )
                         | (
-                            (s.i_category == "Men")
-                            & s.i_color.isin(("forest", "ghost"))
-                            & s.i_units.isin(("Lb", "Bundle"))
-                            & s.i_size.isin(("medium", "extra large"))
+                            (i1.i_manufact == s.i_manufact)
+                            & (
+                                (
+                                    (s.i_category == "Women")
+                                    & s.i_color.isin(("midnight", "snow"))
+                                    & s.i_units.isin(("Pallet", "Gross"))
+                                    & s.i_size.isin(("medium", "extra large"))
+                                )
+                                | (
+                                    (s.i_category == "Women")
+                                    & s.i_color.isin(("cyan", "papaya"))
+                                    & s.i_units.isin(("Cup", "Dram"))
+                                    & s.i_size.isin(("N/A", "small"))
+                                )
+                                | (
+                                    (s.i_category == "Men")
+                                    & s.i_color.isin(("orange", "frosted"))
+                                    & s.i_units.isin(("Each", "Tbl"))
+                                    & s.i_size.isin(("petite", "petite"))
+                                )
+                                | (
+                                    (s.i_category == "Men")
+                                    & s.i_color.isin(("forest", "ghost"))
+                                    & s.i_units.isin(("Lb", "Bundle"))
+                                    & s.i_size.isin(("medium", "extra large"))
+                                )
+                            )
                         )
                     )
-                )
-            ).count()
-            > 0,
+                ).count()
+                > 0
+            ),
         )
         .select(_.i_product_name)
         .distinct()
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L4491'>ibis/backends/tests/tpc/ds/test_queries.py~L4491</a>
```diff
         customer_total_return.join(customer, [("ctr_customer_sk", "c_customer_sk")])
         .join(customer_address, [("c_current_addr_sk", "ca_address_sk")])
         .filter(
-            lambda ctr1: ctr1.ctr_total_return
-            > (
-                ctr2.filter(ctr1.ctr_state == _.ctr_state).ctr_total_return.mean() * 1.2
-            ).as_scalar(),
+            lambda ctr1: (
+                ctr1.ctr_total_return
+                > (
+                    ctr2.filter(ctr1.ctr_state == _.ctr_state).ctr_total_return.mean()
+                    * 1.2
+                ).as_scalar()
+            ),
             _.ca_state == "GA",
         )
         .select(
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/backends/tests/tpc/ds/test_queries.py#L4913'>ibis/backends/tests/tpc/ds/test_queries.py~L4913</a>
```diff
         .filter(
             _.i_manufact_id == 350,
             _.d_date.between(date("2000-01-07"), date("2000-04-26")),
-            lambda t: t.ws_ext_discount_amt
-            > (
-                web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
-                .filter(
-                    t.i_item_sk == _.ws_item_sk,
-                    _.d_date.between(date("2000-01-07"), date("2000-04-26")),
+            lambda t: (
+                t.ws_ext_discount_amt
+                > (
+                    web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
+                    .filter(
+                        t.i_item_sk == _.ws_item_sk,
+                        _.d_date.between(date("2000-01-07"), date("2000-04-26")),
+                    )
+                    .ws_ext_discount_amt.mean()
+                    .as_scalar()
+                    * 1.3
                 )
-                .ws_ext_discount_amt.mean()
-                .as_scalar()
-                * 1.3
             ),
         )
         .select(_.ws_ext_discount_amt.sum().name("Excess Discount Amount"))
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/tests/benchmarks/test_benchmarks.py#L692'>ibis/tests/benchmarks/test_benchmarks.py~L692</a>
```diff
     N = 20_000_000
 
     path = str(tmp_path_factory.mktemp("duckdb") / "data.ddb")
-    sql = (
-        lambda var, table, n=N: f"""
+    sql = lambda var, table, n=N: (
+        f"""
         CREATE TABLE {table} AS
         SELECT ROW_NUMBER() OVER () AS id, {var}
         FROM (
```
<a href='https://github.com/ibis-project/ibis/blob/9126733b38e1c92f6e787f92dc9954e88ab6400d/ibis/tests/expr/test_value_exprs.py#L926'>ibis/tests/expr/test_value_exprs.py~L926</a>
```diff
         operator.gt,
         operator.ge,
         lambda left, right: ibis.timestamp("2017-04-01 00:02:34").between(left, right),
-        lambda left, right: ibis.timestamp("2017-04-01")
-        .cast(dt.date)
-        .between(left, right),
+        lambda left, right: (
+            ibis.timestamp("2017-04-01").cast(dt.date).between(left, right)
+        ),
     ],
 )
 def test_string_temporal_compare(op, left, right):
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+46 -20 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L53'>libs/core/tests/unit_tests/runnables/test_history.py~L53</a>
```diff
 
 def test_input_messages() -> None:
     runnable = RunnableLambda(
-        lambda messages: "you said: "
-        + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        lambda messages: (
+            "you said: "
+            + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        )
     )
     store: dict = {}
     get_session_history = _get_get_session_history(store=store)
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L82'>libs/core/tests/unit_tests/runnables/test_history.py~L82</a>
```diff
 
 async def test_input_messages_async() -> None:
     runnable = RunnableLambda(
-        lambda messages: "you said: "
-        + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        lambda messages: (
+            "you said: "
+            + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        )
     )
     store: dict = {}
     get_session_history = _get_get_session_history(store=store)
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L113'>libs/core/tests/unit_tests/runnables/test_history.py~L113</a>
```diff
 
 def test_input_dict() -> None:
     runnable = RunnableLambda(
-        lambda params: "you said: "
-        + "\n".join(
-            str(m.content) for m in params["messages"] if isinstance(m, HumanMessage)
+        lambda params: (
+            "you said: "
+            + "\n".join(
+                str(m.content)
+                for m in params["messages"]
+                if isinstance(m, HumanMessage)
+            )
         )
     )
     get_session_history = _get_get_session_history()
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L133'>libs/core/tests/unit_tests/runnables/test_history.py~L133</a>
```diff
 
 async def test_input_dict_async() -> None:
     runnable = RunnableLambda(
-        lambda params: "you said: "
-        + "\n".join(
-            str(m.content) for m in params["messages"] if isinstance(m, HumanMessage)
+        lambda params: (
+            "you said: "
+            + "\n".join(
+                str(m.content)
+                for m in params["messages"]
+                if isinstance(m, HumanMessage)
+            )
         )
     )
     get_session_history = _get_get_session_history()
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L155'>libs/core/tests/unit_tests/runnables/test_history.py~L155</a>
```diff
 
 def test_input_dict_with_history_key() -> None:
     runnable = RunnableLambda(
-        lambda params: "you said: "
-        + "\n".join(
-            [str(m.content) for m in params["history"] if isinstance(m, HumanMessage)]
-            + [params["input"]]
+        lambda params: (
+            "you said: "
+            + "\n".join(
+                [
+                    str(m.content)
+                    for m in params["history"]
+                    if isinstance(m, HumanMessage)
+                ]
+                + [params["input"]]
+            )
         )
     )
     get_session_history = _get_get_session_history()
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L177'>libs/core/tests/unit_tests/runnables/test_history.py~L177</a>
```diff
 
 async def test_input_dict_with_history_key_async() -> None:
     runnable = RunnableLambda(
-        lambda params: "you said: "
-        + "\n".join(
-            [str(m.content) for m in params["history"] if isinstance(m, HumanMessage)]
-            + [params["input"]]
+        lambda params: (
+            "you said: "
+            + "\n".join(
+                [
+                    str(m.content)
+                    for m in params["history"]
+                    if isinstance(m, HumanMessage)
+                ]
+                + [params["input"]]
+            )
         )
     )
     get_session_history = _get_get_session_history()
```
<a href='https://github.com/langchain-ai/langchain/blob/57ff48e62e5753de9200fe83adbdb5ac59587e29/libs/core/tests/unit_tests/runnables/test_history.py#L827'>libs/core/tests/unit_tests/runnables/test_history.py~L827</a>
```diff
 
 def test_get_output_messages_no_value_error() -> None:
     runnable = _RunnableLambdaWithRaiseError(
-        lambda messages: "you said: "
-        + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        lambda messages: (
+            "you said: "
+            + "\n".join(str(m.content) for m in messages if isinstance(m, HumanMessage))
+        )
     )
     store: dict = {}
     get_session_history = _get_get_session_history(store=store)
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+6 -4 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/c0a2f87e7a6264f6fb356c1e40ff4e4e8b41d6a5/mlflow/store/model_registry/file_store.py#L895'>mlflow/store/model_registry/file_store.py~L895</a>
```diff
     def _list_file_model_versions_under_path(self, path) -> list[FileModelVersion]:
       

... (truncated 1844 lines) ...



---

_@ntBre reviewed on 2025-11-11 16:39_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_strings.py.snap`:923 on 2025-11-11 16:39_

This seems bad.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_strings.py.snap`:923 on 2025-11-11 18:51_

I think this was resolved by using the correct `Parenthesize` variant. In the rebase I replaced `IfBreaksOrIfRequired` with `IfBreaksParenthesized` but `IfBreaksParenthesizedNested` seems to be the better choice.

---

_@ntBre reviewed on 2025-11-11 18:51_

---

_Comment by @MichaReiser on 2025-11-12 14:04_

```diff
                  > 0
             ),
             lambda t: (
-                web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
-                .filter(
-                    t.c_customer_sk == web_sales.ws_bill_customer_sk,
-                    _.d_year == 2002,
-                    _.d_moy.between(1, 1 + 3),
+                (
+                    web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
+                    .filter(
+                        t.c_customer_sk == web_sales.ws_bill_customer_sk,
+                        _.d_year == 2002,
+                        _.d_moy.between(1, 1 + 3),
+                    )
+                    .count()
+                    > 0
                 )
-                .count()
-                > 0
-            )
-            | (
-                catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
-                .filter(
-                    t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
-                    _.d_year == 2002,
-                    _.d_moy.between(1, 1 + 3),
+                | (
+                    catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
+                    .filter(
+                        t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
+                        _.d_year == 2002,
+                        _.d_moy.between(1, 1 + 3),
+                    )
+                    .count()
+                    > 0
                 )
-                .count()
-                > 0
             ),
         )
         .group_by(
```

this looks unfortunate

---

_Comment by @ntBre on 2025-11-12 14:19_

This actually seems okay to me? There are two large elements being unioned together, and I think nesting them in another set of parens makes it more clear that they're both in the same lambda. At first I thought I had just added an extra set of parens.

The formatted:

```py
lambda t: (
    (
        web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
        .filter(
            t.c_customer_sk == web_sales.ws_bill_customer_sk,
            _.d_year == 2002,
            _.d_moy.between(1, 1 + 3),
        )
        .count()
        > 0
    )
    | (
        catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
        .filter(
            t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
            _.d_year == 2002,
            _.d_moy.between(1, 1 + 3),
        )
        .count()
        > 0
    )
),
```

and unformatted code for comparison:

```py
lambda t: (
    web_sales.join(date_dim, [("ws_sold_date_sk", "d_date_sk")])
    .filter(
        t.c_customer_sk == web_sales.ws_bill_customer_sk,
        _.d_year == 2002,
        _.d_moy.between(1, 1 + 3),
    )
    .count()
    > 0
)
| (
    catalog_sales.join(date_dim, [("cs_sold_date_sk", "d_date_sk")])
    .filter(
        t.c_customer_sk == catalog_sales.cs_ship_customer_sk,
        _.d_year == 2002,
        _.d_moy.between(1, 1 + 3),
    )
    .count()
    > 0
),
```

---

_Comment by @MichaReiser on 2025-11-12 14:43_

Oh, I guess I don't like the parentheses around the union elements but they were already present before

---

_Comment by @ntBre on 2025-11-12 15:33_

I reverted the indentation changes and tried throwing in `RemoveSoftLinesBuffer`. It was rearranging trailing comments on the parameters, so I just skipped the new format if there are _any_ comments present for now, but I assume there's a better approach here, possibly the arbitrary line length you mentioned on Discord.

But the current state of this branch at least resolves the initial deviation from Black reported in https://github.com/astral-sh/ruff/issues/8179.

---

_@ntBre reviewed on 2025-11-12 15:35_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:2016 on 2025-11-12 15:35_

Wrapping the body here is a bit silly since the `(` and the `d` have the same length. But this seems like it should be quite rare in real code.

---

_Comment by @ntBre on 2025-11-12 17:18_

### Ecosystem results

Most of them look good with a few exceptions:

- [providers/google/tests/unit/google/cloud/hooks/test_gcs.py~L420](https://github.com/apache/airflow/blob/0534b90db660d6815be28d7d4b086b4c87df8995/providers/google/tests/unit/google/cloud/hooks/test_gcs.py#L420)

	```diff
	         mock_copy.return_value = storage.Blob(
	             name=destination_object_name, bucket=storage.Bucket(mock_service, destination_bucket_name)
	         )
	-        mock_service.return_value.bucket.side_effect = lambda name: (
	-            source_bucket
	-            if name == source_bucket_name
	-            else storage.Bucket(mock_service, destination_bucket_name)
	+        mock_service.return_value.bucket.side_effect = (
	+            lambda name: (
	+                source_bucket
	+                if name == source_bucket_name
	+                else storage.Bucket(mock_service, destination_bucket_name)
	+            )
	         )
	 
	         self.gcs_hook.copy(
	```
	
	It seems like this should have been fine before? The very last astropy example is like this too.

- [providers/http/tests/unit/http/sensors/test_http.py~L302](https://github.com/apache/airflow/blob/0534b90db660d6815be28d7d4b086b4c87df8995/providers/http/tests/unit/http/sensors/test_http.py#L302)

	```diff
	             method="GET",
	             endpoint="/search",
	             data={"client": "ubuntu", "q": "airflow"},
	-            response_check=lambda response: ("apache/airflow" in response.text),
	+            response_check=lambda response: "apache/airflow" in response.text,
	             headers={},
	         )
	         op.execute({})
	```
	
	I think this is okay, just worth pointing out that we also remove parentheses if the body doesn't wrap.

- [tests/unit_tests/importexport/api_test.py~L48](https://github.com/apache/superset/blob/0b535b792e9bf95dd72f4d9a98a49fbfc04b23f8/tests/unit_tests/importexport/api_test.py#L48)

	```diff
	     mocked_export_result = [
	         (
	             "metadata.yaml",
	-            lambda: "version: 1.0.0\ntype: assets\ntimestamp: '2022-01-01T00:00:00+00:00'\n",  # noqa: E501
	+            lambda: (
	+                "version: 1.0.0\ntype: assets\ntimestamp: '2022-01-01T00:00:00+00:00'\n"
	+            ),  # noqa: E501
	         ),
	         ("databases/example.yaml", lambda: "<DATABASE CONTENTS>"),
	     ]
	```
	
	This seems bad, it breaks the noqa comment (although it also fixes E501 in this case). Maybe this is expected, there's another case where we move the noqa comment in the stable formatting
	
	<details><summary>Here</summary>
	
	 - [rotkehlchen/rotkehlchen.py~L617](https://github.com/rotki/rotki/blob/d3afc02b022c19c4a85984d758e28a9f0dfa1fea/rotkehlchen/rotkehlchen.py#L617)

	``` diff
	                         ]
	                     },  # noqa: E501
	                 ),
	-                extra_check_callback=lambda: cursor.execute(
	-                    "SELECT COUNT(*) FROM user_added_solana_tokens"
	-                ).fetchone()[0]
	-                > 0,  # noqa: E501
	+                extra_check_callback=lambda: (
	+                    cursor.execute("SELECT COUNT(*) FROM user_added_solana_tokens").fetchone()[0]
	+                    > 0
	+                ),  # noqa: E501
	             )
	 
	     def _logout(self) -> None:
	```
	
	</details>

- [ibis/tests/benchmarks/test_benchmarks.py~L693](https://github.com/ibis-project/ibis/blob/bb42a6bd090c5c500ca86303f37bfd66d05767c7/ibis/tests/benchmarks/test_benchmarks.py#L693)

 	```diff
	     path = str(tmp_path_factory.mktemp("duckdb") / "data.ddb")
	     sql = (
	-        lambda var, table, n=N: f"""
	+        lambda var, table, n=N: (
	+            f"""
	         CREATE TABLE {table} AS
	         SELECT ROW_NUMBER() OVER () AS id, {var}
	         FROM (
	```
	
	This seems a bit questionable. The `f"""` feels like it could take the place of parentheses.

- [rotkehlchen/chain/solana/node_inquirer.py~L354](https://github.com/rotki/rotki/blob/d3afc02b022c19c4a85984d758e28a9f0dfa1fea/rotkehlchen/chain/solana/node_inquirer.py#L354)

	``` diff
	         signatures = []
	         while True:
	             response: GetSignaturesForAddressResp = self.query(
	-                method=lambda client,
	-                _before=before,
	-                _until=until: client.get_signatures_for_address(  # type: ignore[misc]  # noqa: E501
	+                method=lambda client, _before=before, _until=until: client.get_signatures_for_address(  # type: ignore[misc]  # noqa: E501
	                     account=Pubkey.from_string(address),
	                     limit=SIGNATURES_PAGE_SIZE,
	                     before=_before,
	```
	
	This case actually seems to be what the project wants because of the `noqa` comment, but I'm pretty sure this line is just over their configured line length of 99. Maybe the `has_own_parentheses` check is too lax here?

---

_Renamed from "[WIP] Indent lambda parameters if parameters wrap" to "[WIP] Keep lambda parameters on one line and parenthesize the body if it expands" by @ntBre on 2025-11-12 17:31_

---

_Renamed from "[WIP] Keep lambda parameters on one line and parenthesize the body if it expands" to "Keep lambda parameters on one line and parenthesize the body if it expands" by @ntBre on 2025-11-14 13:52_

---

_Comment by @ntBre on 2025-11-14 13:54_

Not sure this is exactly ready for review, but the code itself seems fine and I'm curious to get some thoughts on the ecosystem results in https://github.com/astral-sh/ruff/pull/21385#issuecomment-3523043238. I think the results look good overall.

---

_Marked ready for review by @ntBre on 2025-11-14 13:54_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-14 13:54_

---

_Review requested from @amyreese by @MichaReiser on 2025-11-14 13:56_

---

_@MichaReiser reviewed on 2025-11-14 15:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:1690 on 2025-11-14 15:06_

The wrapping here is unfortunate. But it's probably not worth special casing, given that it's a very contrived exmaple

---

_@MichaReiser reviewed on 2025-11-14 15:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:1707 on 2025-11-14 15:06_

This feels worse

---

_Comment by @MichaReiser on 2025-11-14 15:09_

> [providers/google/tests/unit/google/cloud/hooks/test_gcs.py~L420](https://github.com/apache/airflow/blob/0534b90db660d6815be28d7d4b086b4c87df8995/providers/google/tests/unit/google/cloud/hooks/test_gcs.py#L420)

Yeah, this one looks just wrong

---

_@ntBre reviewed on 2025-11-14 15:17_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:1707 on 2025-11-14 15:17_

Ah yeah, this is like the case I flagged in https://github.com/astral-sh/ruff/pull/21385#discussion_r2518795764. But this also seems pretty contrived to me. It seems very unlikely to have a long lambda with a single-character body where the `(` and body are the same length. Unless you think it would be worse for a longer name too.

---

_Comment by @MichaReiser on 2025-11-14 15:28_

I think you want to experiment with hitting this branch

https://github.com/astral-sh/ruff/blob/32d7d52d2af7591cf0d03c01aee319693b54cfb3/crates/ruff_python_formatter/src/expression/parentheses.rs#L196-L205

To improve formatting of 

```
def test():
    def more():
        mock_service.return_value.bucket.side_effect = lambda name: (
            source_bucket
            if name == source_bucket_name
            else storage.Bucket(mock_service, destination_bucket_name)
         )
```

I don't remember the exact semantics but the idea would be to fit the entire lambda in a `fits_expanded`. I haven't thought it through carefully of what the implication of this is for lambda usages in other places than assignments

Edit: Not the entire lambda, only the body part

---

_Converted to draft by @ntBre on 2025-11-18 19:15_

---

_Comment by @codspeed-hq[bot] on 2025-11-19 17:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Findent-lambda-params?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21385 will **not alter performance**

<sub>Comparing <code>brent/indent-lambda-params</code> (95301b3) with <code>main</code> (3ac58b4)</sub>



### Summary

`✅ 30` untouched  
`⏩ 22` skipped[^skipped]  



[^skipped]: 22 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/brent%2Findent-lambda-params?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Renamed from "Keep lambda parameters on one line and parenthesize the body if it expands" to "Parenthesize long lambda bodies" by @ntBre on 2025-11-20 17:50_

---

_Renamed from "Parenthesize long lambda bodies" to "Keep lambda parameters on one line and parenthesize the body if it expands" by @ntBre on 2025-11-20 17:58_

---

_Comment by @ntBre on 2025-11-20 23:06_

I still see at least one problem in the ecosystem report, but the results are looking better, and I rebased to clean up the history a bit.

---

_@MichaReiser reviewed on 2025-11-21 07:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:99 on 2025-11-21 07:34_

Can you try `parenthesized` instead of `optional_parentheses`. 

```suggestion
                    fits_expanded(&parenthesized("(", &body.format(), ")")).fmt(f)
```

---

_@ntBre reviewed on 2025-11-21 20:03_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:99 on 2025-11-21 20:03_

It looks like this always parenthesizes the body, one example:

```diff
#### Preview changes            
```diff                         
--- Stable                      
+++ Preview                     
@@ -23,8 +23,8 @@               
     pass                       
                                
                                
-x1 = lambda y: 1               
-x2 = lambda y,: 1              
+x1 = lambda y: (1)             
+x2 = lambda y,: (1)            
                                
 # Ignore trailing comma.       
 with a:  # magic trailing comma
```

I also tried inverting the order (`optional_parentheses(fits_expanded(...))`) like we talked about, but it then doesn't wrap the f-string case:

```diff
-msg = lambda x: (                                                                                       
-    f"this is a very very very very long lambda value {x} that doesn't fit on a single line"            
-)                                                                                                       
+msg = lambda x: f"this is a very very very very long lambda value {x} that doesn't fit on a single line"
```

That seems to be the only test failure, though.

---

_@ntBre reviewed on 2025-11-21 20:09_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/builders.rs`:1 on 2025-11-21 20:09_

Oh no, I thought I had reverted this, but I must have missed it in the rebase. I guess I'm not as close as I thought.

---

_@MichaReiser reviewed on 2025-11-22 13:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:99 on 2025-11-22 13:58_

ah sorry, I meant `parenthesize_if_expands`

We can't use `fits_expanded` because it's causing this weird wrapping here where the binary expression continues on the same line

```patch
         # If it has been indicated that the user wants the interpolator, construct
         # an anonymous function to handle units and sanitize IO
         if return_interpolator:
-            return (
-                lambda x: np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
-            )
+            return lambda x: np.exp(
+                cs(np.log(x.to(u.MeV).value))
+            ) * u.MeV * u.cm**2 / u.g
 
         return (
             energies,
```

---

_@ntBre reviewed on 2025-11-22 14:38_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:99 on 2025-11-22 14:38_

Ahh, thank you! That's the case I added in the last commit and looked at yesterday. I'll try `parenthesize_if_expands`.

---

_Comment by @ntBre on 2025-12-01 17:21_

Thanks again for the `parenthesize_if_expands` suggestion, that cleaned up the ecosystem issue you pointed out.

The other ecosystem changes now look good to me, with the exception of the last two cases I mentioned before in https://github.com/astral-sh/ruff/pull/21385#issuecomment-3523043238, which I've now added as tests. These get formatted like this:

```py
class C:
    def foo():
        if True:
            transaction_count = self._query_txs_for_range(
                get_count_fn=lambda from_ts, to_ts, _chain_id=chain_id: db_evmtx.count_transactions_in_range(
                    chain_id=_chain_id,
                    from_ts=from_ts,
                    to_ts=to_ts,
                ),
            )

def ddb():
    sql = lambda var, table, n=N: (
        f"""
        CREATE TABLE {table} AS
        SELECT ROW_NUMBER() OVER () AS id, {var}
        FROM (
            SELECT {var}
            FROM RANGE({n}) _ ({var})
            ORDER BY RANDOM()
        )
        """
    )
```

where the lambda body in the first case exceeds the configured line length, and in the second case, I would expect not to have an extra set of parentheses around the f-string. In other words I would expect this output:

```py
class C:
    def foo():
        if True:
            transaction_count = self._query_txs_for_range(
                get_count_fn=lambda from_ts, to_ts, _chain_id=chain_id: (
                    db_evmtx.count_transactions_in_range(
                        chain_id=_chain_id,
                        from_ts=from_ts,
                        to_ts=to_ts,
                    )
                ),
            )

def ddb():
    sql = lambda var, table, n=N: f"""
        CREATE TABLE {table} AS
        SELECT ROW_NUMBER() OVER () AS id, {var}
        FROM (
            SELECT {var}
            FROM RANGE({n}) _ ({var})
            ORDER BY RANDOM()
        )
    """
```

with this diff between the two:

```diff
5,8c5,10
<                 get_count_fn=lambda from_ts, to_ts, _chain_id=chain_id: db_evmtx.count_transactions_in_range(
<                     chain_id=_chain_id,
<                     from_ts=from_ts,
<                     to_ts=to_ts,
---
>                 get_count_fn=lambda from_ts, to_ts, _chain_id=chain_id: (
>                     db_evmtx.count_transactions_in_range(
>                         chain_id=_chain_id,
>                         from_ts=from_ts,
>                         to_ts=to_ts,
>                     )
14,15c16
<     sql = lambda var, table, n=N: (
<         f"""
---
>     sql = lambda var, table, n=N: f"""
23,24c24
<         """
<     )
---
>     """
```

I'm kind of okay with both of these, and vaguely remember that we might have already discussed them too. In the first case, the project already has `noqa: E501` comments, so I think this is actually the formatting they want, and in the latter case we at least don't add any extra parentheses compared to the [stable formatting](https://play.ruff.rs/9eaba5d4-44aa-43c6-a946-d5dd494624c7?secondary=Format), which parenthesizes the whole lambda.

But the f-string case also (naively) seems easier to fix than the call, which `has_own_parentheses` returns `Some` for.

So this may be ready for review again, depending on how we want to handle those cases and if my changes to `ParenthesizeIfExpands` are acceptable.

---

_Comment by @amyreese on 2025-12-01 17:57_

Personally I actually prefer parentheses around the f-string in your example, because that allows the opening quotes to match indentation of the closing quotes.

---

_@MichaReiser reviewed on 2025-12-02 09:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:1 on 2025-12-02 09:51_

Can you revert those changes before opening this for review?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:106 on 2025-12-02 09:52_

I think this should be the other way round
```suggestion
                    parenthesize_if_expands(&fits_expanded(&body.format())).fmt(f)
```

---

_@MichaReiser reviewed on 2025-12-02 09:52_

---

_@MichaReiser reviewed on 2025-12-02 09:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:424 on 2025-12-02 09:53_

Hmm, `maybe_parenthesize_expression` shouldn't contain any expression specific formattingl Can you say more why this change is necessary?

---

_@MichaReiser reviewed on 2025-12-02 09:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:312 on 2025-12-02 09:54_

We shouldn't use `maybe_parenthesize_expression` if we see a lambda, instead, call the lambda formatting directly.

Just make sure you add some tests for when the lambda is parenthesized and has comments

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/builders.rs`:1 on 2025-12-02 14:08_

I've tried reverting this a couple of times, but it introduces instability in at least these cases:

```diff
       if True:
           if True:
               return lambda x: (
  -                    np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
  -                )
  +                np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
  +            )
   
   
   class C:
  @@ -361,7 +361,7 @@
   
   def ddb():
       sql = lambda var, table, n=N: (
  -            f"""
  +        f"""
           CREATE TABLE {table} AS
           SELECT ROW_NUMBER() OVER () AS id, {var}
           FROM (
  @@ -370,4 +370,4 @@
               ORDER BY RANDOM()
           )
           """
  -        )
  +    )
```

So I will have to find a different way to avoid that if we want to revert this.

---

_@ntBre reviewed on 2025-12-02 14:08_

---

_@ntBre reviewed on 2025-12-02 14:10_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:106 on 2025-12-02 14:10_

Swapping them without other changes introduces a syntax error:

```
       --> /home/brent/astral/ruff/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py:342:13
        |
    340 |             * u.MeV
    341 |             * u.cm**2
    342 |             / u.g
        |             ^
        |
```

on this snippet:

```py
def foo():
    if True:
        if True:
            return (
                lambda x: np.exp(cs(np.log(x.to(u.MeV).value))) * u.MeV * u.cm**2 / u.g
            )
```

This is the output:

```py
    def foo():
        if True:
            if True:
                return lambda x: np.exp(cs(np.log(x.to(u.MeV).value)))
                * u.MeV
                * u.cm**2
                / u.g
```

---

_@ntBre reviewed on 2025-12-02 14:12_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/mod.rs`:424 on 2025-12-02 14:12_

I was trying to propagate the `Layout` setting from the assignment formatting, which doesn't format the lambda directly. It calls `maybe_parenthesize_expression` first.

---

_@ntBre reviewed on 2025-12-02 14:12_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:312 on 2025-12-02 14:12_

Ah okay, this probably resolves your other question about why I was doing this in the first place.

---

_Comment by @ntBre on 2025-12-02 17:24_

I "fixed" (see below) the call formatting by excluding both call and subscript expressions from the `has_own_parentheses` check; and inlined the lambda formatting from `maybe_parenthesize_expression` into the assignment formatting and reverted the `maybe_parenthesize_expression` changes. I also added a few new tests (I tried to get Claude to suggest even more, but they didn't look very interesting beyond what we had already).

For some reason the ecosystem comment isn't updating, but I downloaded the report, and there are actually many more changes now as a result of the `has_own_parentheses` check. I think we're now wrapping call arguments too eagerly. Here's one example:

```diff
             if self.location in EVM_EVMLIKE_LOCATIONS and database is not None:
                 # call _maybe_add_label_with_address on each address in the note
                 exported_dict["notes"] = EVM_ADDRESS_REGEX.sub(
-                    repl=lambda matched_address: self._maybe_add_label_with_address(
-                        database=database,
-                        matched_address=matched_address,
+                    repl=lambda matched_address: (
+                        self._maybe_add_label_with_address(
+                            database=database,
+                            matched_address=matched_address,
+                        )
                     ),
                     string=exported_dict["notes"],  # type: ignore [call-overload]  # exported_dict['notes'] is always a string
                 )
```

The initial code is only 83 columns wide. Is that desirable, or do I need to try to find a more careful check? I think I would lean toward reverting this change and accepting the occasional overly-long lambda header, if these are our only two choices. That was much, much less common in the ecosystem, and the cases with many parameters plus a long callable seemed to be intentionally formatted that way in the original code.

<hr>

As noted above, I believe the changes to `parenthesize_if_expands` are necessary to avoid instability. This is probably the clearest example, which came up earlier:

```diff
 msg = lambda x: (                                                                               
-        f"this is a very very very very long lambda value {x} that doesn't fit on a single line"
-    )                                                                                           
+    f"this is a very very very very long lambda value {x} that doesn't fit on a single line"    
+)                                                                                               
```

Note the indented parenthesis in the first format (the `-` lines). This is from the unconditional `soft_block_indent` in the old version of `parenthesize_if_expands`. I think using `indent_if_group_breaks` like we do in `optional_parentheses` is correct, and I haven't seen any non-lambda changes in the ecosystem report.

Of course, I could be missing a better way to avoid this, so I'm happy to revisit it, but I currently believe it to be necessary, in contrast to what I said initially (https://github.com/astral-sh/ruff/pull/21385#discussion_r2550845179).

---

_Comment by @MichaReiser on 2025-12-02 19:08_

I'll take a look tomorrow. I want to understand why the lambda doesn't get wrapped in that instance. Might it just be that we hit a case where `needs_parentheses` returns `Never` here

https://github.com/astral-sh/ruff/blob/95c61deb8a9a42e8c9a8626f98d440ffb7652ea5/crates/ruff_python_formatter/src/expression/expr_attribute.rs#L196

And all that we need to change (using `maybe_parenthesize_expression` again) is to change `needs_parentheses` of call expression formatting to return `BestFit` or `Multiline` if it is the body of a lambda expression? Same for subscript?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:59 on 2025-12-02 19:15_

Use write
```suggestion
                        write!(f, [
                        	if_group_breaks(&token("(")).
                        	Arguments::from(&self.inner);
                        	if_group_breaks(&token(")"))
                      	])?;
                    }
```

We only use `.fmt` when formatting a single item

---

_@MichaReiser reviewed on 2025-12-02 19:15_

---

_@MichaReiser reviewed on 2025-12-02 19:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:34 on 2025-12-02 19:16_

We should avoid creating the id if `self.indent` is false. We should also use a better unique name

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:40 on 2025-12-02 19:17_

We're now adding two groups. One outer group here and an inner group within `self.indent`. This seems wrong to me and extra groups can be very costly

---

_@MichaReiser reviewed on 2025-12-02 19:17_

---

_Comment by @ntBre on 2025-12-02 20:47_

> I'll take a look tomorrow. I want to understand why the lambda doesn't get wrapped in that instance. Might it just be that we hit a case where `needs_parentheses` returns `Never` here
> 
> https://github.com/astral-sh/ruff/blob/95c61deb8a9a42e8c9a8626f98d440ffb7652ea5/crates/ruff_python_formatter/src/expression/expr_attribute.rs#L196
> 
> And all that we need to change (using `maybe_parenthesize_expression` again) is to change `needs_parentheses` of call expression formatting to return `BestFit` or `Multiline` if it is the body of a lambda expression? Same for subscript?

I looked into this briefly by debugging the return value of `ExprCall::needs_parentheses` for the case above, and it was already returning `BestFit` via the recursive `self.func.needs_parentheses` branch.

---

_Comment by @ntBre on 2025-12-02 21:08_

Oh, I just realized you meant in the original code when it wasn't wrapping the long one. I'll check that too.

---

_@MichaReiser reviewed on 2025-12-03 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:96 on 2025-12-03 09:18_

What's the reason we skip over expressions that are parenthesized? This has the issue that the new formatting style isn't reversible in the sense that Ruff might introduce new parentheses but will fail to remove the parentheses once they're no longer needed. 



---

_Comment by @MichaReiser on 2025-12-03 10:03_

Looking at 

```
class C:
    def foo():
        if True:
            transaction_count = self._query_txs_for_range(
                get_count_fn=lambda from_ts, to_ts, _chain_id=chain_id: db_evmtx.count_transactions_in_range(
                    chain_id=_chain_id,
                    from_ts=from_ts,
                    to_ts=to_ts,
                ),
            )
```


Which gives you the following IR:

```
		...
		"self._query_txs_for_range(",
          fits_expanded(propagate_expand: true, condition: if_group_fits_on_line("#optional_parentheses-2"), [
            group(expand: propagated, [
              indent([
                soft_line_break,
                group(expand: propagated, [
                  "get_count_fn=lambda ",
                  group(["from_ts, to_ts, _chain_id=chain_id"]),
                  ": db_evmtx.count_transactions_in_range(",
                  group(expand: propagated, [
                    indent([
                      soft_line_break,
                      group(expand: propagated, [
                        "chain_id=_chain_id,",
                        soft_line_break_or_space,
                        "from_ts=from_ts,",
                        soft_line_break_or_space,
                        "to_ts=to_ts",
                        if_group_breaks([","]),
                        expand_parent
                      ])
                    ]),
                    soft_line_break
                  ]),
                  ")",
                  if_group_breaks([","]),
                  expand_parent
                ])
              ]),
              soft_line_break
            ])
          ]),
		  ...
```

The important IR element is `": db_evmtx.count_transactions_in_range(",`, note how the formatter doesn't emit the IR for optionally parenthesizing the lambda body. It always assumes that it never needs parentheses. This suggests that `ExprCall::needs_parentheses` returns `Never`. I worry that we'll have the same problem with many other expressions that return `Never` because they don't want to be parenthesized in a clause header, but we now use `maybe_parenthesize_expression` in a context where we still want to parenthesize those expressions. While we can try to fix them up, to me it feels as if we're mixing concerns here and using `maybe_parenthesize_expression` here might not be a good idea after all. It's also the first use of `maybe_parenthesize_expression` within a nested expression (ignoring `await` and `yield` which are often used like statements.

Overall, this doesn't give me confidence that using `maybe_parenthesize_expression` is the right tool here. It's tempting to use it here because it almost gives us what we want, but without us understanding why it works (or even knowing what exactly we want?).


Thinking through the body formatting logic:

* If the item has its own parentheses (ignoring subscript and call), we should never add parentheses unless the value requires it because it has leading or trailing comments, but I think that `Expr::format` already handles this for us. Therefore, we can simply call `body.format` without adding any parentheses in that case
* For all other expressions other than subscript and call, we simply use `parenthesize_if_expands(body.format)` (ignoring the assignment formatting for now)
* Now, Subscript and `Call` are tricky, because we only want to add parentheses if the part coming before the `(` doesn't fit. I would expect that we can use `fits_expanded`, the same as we do for the assignment case but this doesn't seem to work for reasons I don't fully understand. A simplified version of the case is:
	```py
    lambda from_ts, to_ts, _chain_id=chain_id: db_evmtx.count_(
        chain_id=_chain_id,
        from_ts=from_ts,
        to_ts=to_ts
    )
	```


After debugging printer, the issue is that `fits_expanded` only prevents splitting `parameters` if splitting the `db_evmtx.count_` prevents makes the expression fit on a single line. But what we need is that the call expression formatting adds a `fits_expanded` around its call arguments so that we don't parenthesize `db_evmtx.count_(` if expanding the arguments make the expression fit. Luckily, `FormatCallExpr` supports just that if the lambda body uses `optional_parentheses` instead of `parenthesize_if_expands`.

Changing the formatting logic to the following almost gives us what we want:

```rust
        if is_parenthesize_lambda_bodies_enabled(f.context()) {
            let fmt_body = format_with(|f| {
                if matches!(&**body, Expr::Call(_) | Expr::Subscript(_)) {
                    optional_parentheses(
                        &body
                            .format()
                            .with_options(crate::expression::parentheses::Parentheses::Never),
                    )
                    .fmt(f)
                } else if has_own_parentheses(body, f.context()).is_some() {
                    // We probably need to be more careful here and preserve parentheses
                    body.format().fmt(f)
                } else {
                    // Can we always use `optional_parentheses`?
                    parenthesize_if_expands(
                        &body
                            .format()
                            .with_options(crate::expression::parentheses::Parentheses::Never),
                    )
                    .fmt(f)
                }
            });

            match self.layout {
                ExprLambdaLayout::Assignment => fits_expanded(&fmt_body).fmt(f),
                ExprLambdaLayout::Default => fmt_body.fmt(f),
            }
        } else {
            body.format().fmt(f)
        }
```

However, this doesn't play nicely with lambdas nested within other expressions:


```
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
      206   206 │ +).casefold() == ([]).unicodedata.normalize("NFKCNFKCNFKCNFKCNFKC", s2).casefold()
      207   207 │
      208   208 │  return await self.http_client.fetch(
      209   209 │      f"http://127.0.0.1:{self.port}{path}",
            210 │+@@ -88,6 +88,6 @@
            211 │+     )
            212 │+ )
            213 │+
            214 │+-response = await sync_to_async(
            215 │+-    lambda: self.django_handler.get_response(request), thread_sensitive=True
            216 │+-)()
            217 │++response = await sync_to_async(lambda: self.django_handler.get_response(
            218 │++        request
            219 │++    ), thread_sensitive=True)()
      210   220 │ ```
    ────────────┴───────────────────────────────────────────────────────────────────
```


The printer now sees that everything up to the `(` of the `get_response` call expression fits, so that we don't indent the body of `sync_to_async`. But that's obviously not what we want. We want the formatter to indent the call argument, but not the lambda body. 



Even when using `maybe_parenthesize_expression`, there are cases where the formatter breaks before the opening `(`, because it doesn't use `OptionalParentheses::Never` (the subscript could break:


```py
class C:
    def foo():
        if True:
            transaction_count = self._query_txs_for_range(
                lambda from_ts, to_ts, _chain_id=chain_id: (
                    db_evmtex[more].call(
                        chain_id=_chain_id,
                        from_ts=from_ts,
                        to_ts=to_ts,
                    )
                ),
            )
```

I'm not sure what the solution here is and I now need to switch to other work but I don't think we've figured that one out yet



---

_@MichaReiser reviewed on 2025-12-03 10:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-03 10:52_

I'm pretty sure `can_omit_optional_parentheses` always returns `False` for a lambda expression

---

_Comment by @MichaReiser on 2025-12-03 11:10_

I think this mostly works? We should avoid using `best_fitting` where we can but here might be an okay enough use case. 

Search for some other usages of `best_fitting` to see what checks we normally perform to bail early if it isn't strictly necessary. 

An alternative would be to implement the `Marker` feature that we talked about a few weeks ago but I think we can re-visit this separately and I don't think performance will suffer to much, given that lambdas tend to be rare. 

```rust
        if is_parenthesize_lambda_bodies_enabled(f.context()) {
            let fmt_body = format_with(|f| {
                if matches!(&**body, Expr::Call(_) | Expr::Subscript(_)) {
                    let body = body
                        .format()
                        .with_options(crate::expression::parentheses::Parentheses::Never)
                        .memoized();

                    best_fitting![
                        // body all flat
                        body,
                        // body expanded
                        body,
                        // parenthesized
                        format_args![token("("), block_indent(&body), token(")")]
                    ]
                    .fmt(f)
                } else if has_own_parentheses(body, f.context()).is_some() {
                    // We probably need to be more careful here and preserve parentheses if there are comments?
                    body.format().fmt(f)
                } else {
                    parenthesize_if_expands(
                        &body
                            .format()
                            .with_options(crate::expression::parentheses::Parentheses::Never),
                    )
                    .fmt(f)
                }
            });

            match self.layout {
                // Can we move the `fits_expanded` into the assignment formatting?
                ExprLambdaLayout::Assignment => fits_expanded(&fmt_body).fmt(f),
                ExprLambdaLayout::Default => fmt_body.fmt(f),
            }
        } else {
            body.format().fmt(f)
        }
```

---

_@ntBre reviewed on 2025-12-03 14:04_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:96 on 2025-12-03 14:04_

That was for another instability/duplicate parentheses. Here's one example instability after removing it:

```diff
 msg = lambda x: (                                                                               
-    f"this is a very very very very long lambda value {x} that doesn't fit on a single line"    
+    (                                                                                           
+        f"this is a very very very very long lambda value {x} that doesn't fit on a single line"
+    )                                                                                           
 )                                                                                               
```

Maybe that's another sign this isn't the right approach.

---

_Comment by @ntBre on 2025-12-03 14:51_

Wow, thank you! That patch does seem to resolve everything. I'm still looking at the other uses of `best_fitting` to see how we could bail out early. Are there supposed to be two different entries for the flat and expanded cases here:

```rust
                    best_fitting![
                        // body all flat
                        body,
                        // body expanded
                        body,
                        // parenthesized
                        format_args![token("("), block_indent(&body), token(")")]
                    ]
```

or should I just delete one of them? Everything seems to work either way, at least on our current test cases.

You also fixed a Black deviation in the multi-line string preview style!

---

_@ntBre reviewed on 2025-12-03 19:26_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-03 19:26_

I threw a `panic!` in this branch, and it's definitely being entered. Not sure under what conditions, though.

---

_Comment by @ntBre on 2025-12-03 20:28_

Hopefully I'm not missing anything too obvious, but the ecosystem check now looks great to me. A couple of long lines stuck out, but I checked with the projects' configuration, and they were within the configured `line-length`. So it all looks good.

I looked at the other uses of `best_fitting!` and didn't see any great ways for us to bail out earlier, maybe just if `unparenthesized` won't break? I think we could just use the unparenthesized formatting in that case.

I'll mark this ready for review in any case, hopefully that's just a minor point. Thanks Micha yet again for all of the help!



---

_Marked ready for review by @ntBre on 2025-12-03 20:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-03 21:53_

We should add a lot more tests (unless they already exist in some form) for different comment placement, including combinations where the body or lambda are parenthesized (and for every special case branch that we have)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:1 on 2025-12-03 21:54_

Do we still need this 😅?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:110 on 2025-12-03 21:58_

Let's add some documentation to those branches. What's the intended formatting for each of them, eg. For this brmach, the idea is to never add parantheses for expressions that start with a parentheses

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:111 on 2025-12-03 22:00_

Why do we need the contains_comments branch here? If so, I would expect that we need to parenthesize the expression if there are comments.

Contains_commments is also too aggressive. A call-argument comment shouldn't change the layout.

I suggest removing this branch, add tests, then add the minimal necessary comment handling (and documment it)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:120 on 2025-12-03 22:01_

Can we move this into the assignment formatting instead?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:66 on 2025-12-03 22:01_

Do we need both? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-03 22:06_

I would like to understand when it takes this branch and if it's indeed necessary. 

Or is it the opposite and the lambda formatting always takes this branch?

I know it's annoying to narrow this all down but it will be much harder to understand for someone who has to change this code in the future. That's why it's important that the formatting code is as minimal as possible 

---

_@MichaReiser requested changes on 2025-12-03 22:07_

---

_@ntBre reviewed on 2025-12-03 22:09_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-03 22:09_

We have a lot of tests in this file, including with comments in various places. And I didn't bump into anything very interesting when sprinkling more comments into the assignment cases, but I can try injecting more again.

---

_@ntBre reviewed on 2025-12-03 22:10_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/builders.rs`:1 on 2025-12-03 22:10_

Yes, just tried reverting it again, and the formatting is still unstable without it.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:120 on 2025-12-03 22:12_

I wrote this in my commit message for https://github.com/astral-sh/ruff/pull/21385/commits/7dddcc85bc1e3c291e6ea1df1a2e65b91ea34f6b when I deleted your comment, but I don't think we can:

> I don't think we can move the `fits_expanded` call into the assignment
formatting because that would wrap the whole lambda in a `fits_expanded`, when we
just want to wrap the lambda body in it instead. if I understand correctly, we'd
need to duplicate basically this whole function to inject `fits_expanded` in the
right place for the lambda formatting in assignments

I could be missing something obviously.

---

_@ntBre reviewed on 2025-12-03 22:12_

---

_@ntBre reviewed on 2025-12-03 22:15_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-03 22:15_

It panics in the other branch too. I just copied these branches from inside of `maybe_parenthesize_expression` when I moved the layout out of it. I'll try to narrow it down.

---

_@ntBre reviewed on 2025-12-03 22:17_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:111 on 2025-12-03 22:17_

I added this to fix a couple of existing tests altered by the `best_fitting` changes in https://github.com/astral-sh/ruff/pull/21385/commits/6f6c09c72a1ce0b7adf5d46744d1e50d02463d46. It makes sense that it's too aggressive, I'll try to narrow it down.

---

_@MichaReiser reviewed on 2025-12-03 22:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-03 22:17_

It's not just about assignment. We also need tests for all sort of combinations between comments and parenthesized bodies (but maybe they all exist already? I'm on my phone and can't check it)

---

_@MichaReiser reviewed on 2025-12-03 22:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:120 on 2025-12-03 22:18_

I don't think that makes a difference because the header never breaks? Have you tried it?

---

_@ntBre reviewed on 2025-12-03 22:19_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-03 22:19_

I probably phrased that poorly. I just meant nothing went wrong in the assignment cases, so I kind of assumed all comment placement was handled well. I will try injecting more comments into various (non-assignment) cases again.

---

_@ntBre reviewed on 2025-12-03 22:24_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:120 on 2025-12-03 22:24_

I tried it just now with this patch:

```diff
-                        let lambda = lambda.format().with_options(ExprLambdaLayout::Assignment);
+                        let lambda_format = lambda.format();
+                        let lambda = fits_expanded(&lambda_format);
```

and it caused this syntax error:

```
    error[internal-error]: Expected an expression
      --> /home/brent/astral/ruff/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py:57:24
       |
    55 | # Trailing
    56 |
    57 | a = lambda:  # Dangling
       |                        ^
    58 | 1
       |
```

Maybe that's the wrong approach, though.

---

_Converted to draft by @ntBre on 2025-12-04 01:45_

---

_@MichaReiser reviewed on 2025-12-04 08:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-04 08:28_

The comment here might be relevant. Does the formatting change if you remove the optional_parantheses case and how?

https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/src/expression/mod.rs (on my phone, go to can_omit_optional_parentheses and there's a long lambda related comment. Could we remove the lambda special casing in preview?

---

_@MichaReiser reviewed on 2025-12-04 08:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:120 on 2025-12-04 08:30_

I see. Lambda headers can split if they contain comments. Thanks for trying

---

_@ntBre reviewed on 2025-12-04 14:49_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/preview.rs`:66 on 2025-12-04 14:49_

No, I think not. I forgot we discussed not landing the two separately.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:309 on 2025-12-04 15:38_

Ah thank you, yes that comment seems very relevant. I tried removing the `optional_parentheses` branches, and a couple example snapshot failures include:

```diff
-        mock_service.return_value.bucket.side_effect = lambda name: (     
-            source_bucket                                                 
-            if name == source_bucket_name                                 
-            else storage.Bucket(mock_service, destination_bucket_name)    
+        mock_service.return_value.bucket.side_effect = (                  
+            lambda name: (                                                
+                source_bucket                                             
+                if name == source_bucket_name                             
+                else storage.Bucket(mock_service, destination_bucket_name)
+            )                                                             
```

and

```diff
 class C:                                                                         
-    _is_recognized_dtype: Callable[[DtypeObj], bool] = lambda x: lib.is_np_dtype(
-        x, "M"                                                                   
-    ) or isinstance(x, DatetimeTZDtype)                                          
+    _is_recognized_dtype: Callable[[DtypeObj], bool] = (                         
+        lambda x: lib.is_np_dtype(x, "M") or isinstance(x, DatetimeTZDtype)      
+    )                                                                            
```

just like the comment said. But yes, it seems safe to skip this in preview with no snapshot changes.

---

_@ntBre reviewed on 2025-12-04 15:38_

---

_@ntBre reviewed on 2025-12-04 16:49_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:111 on 2025-12-04 16:49_

The two cases that changed before I added this were these:

```diff
-lambda: (  # comment 1 
+lambda: (
+    # comment 1
     x                 
 )                     
                       
@@ -184,7 +167,8 @@    
                       
 (                     
     lambda:  # comment
-    (  # comment 2 
+    (
+        # comment 2
         x             
     )                 
 )                     
```

with these inputs:

```py
lambda: ( # comment 1
    x)

(
    lambda:  # comment
    (  # comment 2
        x
    )
)
```

So I think the narrower condition is something like "has a leading, end-of-line comment within the already-parenthesized body."

Instead of checking comments, we could also use `is_expression_parenthesized` here. That avoids these comment changes too, but it will preserve unnecessary parentheses in cases like this:

```py
lambda xxxxxxxxxxxxxxxxxxxx: (xxxxxxxxxxxxxxxxxxxx + 1)
```

Are we supposed to be removing those? It came up pretty often in the ecosystem check too. I thought it looked nicer to remove them, but maybe we're not supposed to remove parentheses the user included in the input.

---

_@MichaReiser reviewed on 2025-12-04 17:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:111 on 2025-12-04 17:59_

> Are we supposed to be removing those? It came up pretty often in the ecosystem check too. I thought it looked nicer to remove them, but maybe we're not supposed to remove parentheses the user included in the input.

Yes, we're supposed to remove unnecessary parentheses in instances where they might have been introduced by the formatter before. This is important for the formatting to be reversible, something we try all our formatting styles to adhere to (with the exception of `skip-magic-trailing-comma)

I think what you want here is to test if there's any leading or trailing comment on `x`, and if so, call `body.format().with_options(Parentheses::Always)`. Similar to what we have here https://github.com/astral-sh/ruff/blob/bc6331200163546b5db19d04444e51c38db05a3e/crates/ruff_python_formatter/src/expression/mod.rs#L380-L382

---

_@ntBre reviewed on 2025-12-04 23:02_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-04 23:02_

I added a bunch of additional tests, with some help from Claude admittedly. I tried to edit down the redundant ones, but I'm not sure I've captured everything you had in mind. The main interesting cases in the new tests involve dangling comments on the lambda header, which end up outside the body parentheses:

```diff
 (
     lambda name:
     # dangling header comment
-    source_bucket
-    if name == source_bucket_name
-    else storage.Bucket(mock_service, destination_bucket_name)
+    (
+        source_bucket
+        if name == source_bucket_name
+        else storage.Bucket(mock_service, destination_bucket_name)
+    )
 )
```

instead of something more like this:

```py
(
    lambda name: (
        # leading comment
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)
```

but I think this is pretty reasonable and consistent with our unary comment formatting? (This is reminding me a bit of #21501.)

Calls are maybe slightly more awkward since they don't end up parenthesized in this case at all:

```py
(
    lambda from_ts, to_ts, _chain_id=chain_id:
    # dangling header comment before call
    db_evmtx.count_transactions_in_range(
        chain_id=_chain_id,
        from_ts=from_ts,
        to_ts=to_ts,
    )
)
```

This isn't a diff because it doesn't get reformatted. This is the same as our stable formatting.

Another case is a comment within the parameter list. This somewhat obviously prevents collapsing the parameter list to a single line:

```diff
 foo(
     lambda from_ts,  # comment prevents collapsing the parameters to one line
-    to_ts, _chain_id=chain_id: db_evmtx.count_transactions_in_range(
+    to_ts,
+    _chain_id=chain_id: db_evmtx.count_transactions_in_range(
         chain_id=_chain_id,
         from_ts=from_ts,
         to_ts=to_ts,
```

I don't really have a better idea here, but it seemed worth pointing out. Black relocates the comment to the end of the line so that it can join the parameters, but I don't think we want to do that.

I did confirm that we will still wrap and parenthesize the body if it gets too long, at least:

```diff
 foo(
     lambda from_ts,  # but still wrap the body if it gets too long
     to_ts,
-    _chain_id=chain_id: db_evmtx.count_transactions_in_rangeeeeeeeeeeeeeeeeeeeeeeeeeeeee(
-        chain_id=_chain_id,
-        from_ts=from_ts,
-        to_ts=to_ts,
+    _chain_id=chain_id: (
+        db_evmtx.count_transactions_in_rangeeeeeeeeeeeeeeeeeeeeeeeeeeeee(
+            chain_id=_chain_id,
+            from_ts=from_ts,
+            to_ts=to_ts,
+        )
     )
 )
```

I also haven't found any test cases where the `body_comments.has_trailing_own_line()` has any effect. However, deleting that part in the `maybe_parenthesize_expression` code where I copied mine from also doesn't change any snapshots, for whatever that's worth.

---

_@MichaReiser reviewed on 2025-12-05 07:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:144 on 2025-12-05 07:19_

We probably want to make this the first condition as it doesn't make sense to use `best_fitting` if we know the body has comments

---

_@MichaReiser reviewed on 2025-12-05 07:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-05 07:26_

That's a great find. Let's improve our dangling comment formatting and I'm leaning towards always using the `parenthesize_if_expands` layout in that case (for all nodes). I hope that this shouldn't be too hard, given that we already have dangling comment formatting in `ExprLambda`. 

```py
(
     lambda name: # dangling header comment    
    (
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)

# Format to, includding call and subscript expressions
(
     lambda name: ( # dangling header comment    
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)


(
    lambda from_ts, to_ts, _chain_id=chain_id: # dangling header comment
    db_evmtx.count_transactions_in_range(
        chain_id=_chain_id,
        from_ts=from_ts,
        to_ts=to_ts,
    )
)

(
    lambda from_ts, to_ts, _chain_id=chain_id: (  # dangling header comment
        db_evmtx.count_transactions_in_range(
            chain_id=_chain_id,
            from_ts=from_ts,
            to_ts=to_ts,
        )
    )
)

# Do the same for dangling lambda header parameters
(
    lambda # dangling header comment
    from_ts, to_ts, _chain_id=chain_id:
    db_evmtx.count_transactions_in_range(
        chain_id=_chain_id,
        from_ts=from_ts,
        to_ts=to_ts,
    )
)

# Formats to
(
    lambda from_ts, to_ts, _chain_id=chain_id: ( # dangling header comment
        db_evmtx.count_transactions_in_range(
            chain_id=_chain_id,
            from_ts=from_ts,
            to_ts=to_ts,
        )
    )
)
    
# For in-between body:
(
    lambda name: 
    # leading comment
    (
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)

# What we have seems fine, but I could also see us always parenthesizing the value and moving
# formatting it before the body expression
(
    lambda name: 
    (
        # leading comment
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)
```

Placing end-of-line comments at the end of the line makes more sense now, where we flatten the lambda header. 

---

_@ntBre reviewed on 2025-12-05 14:50_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-05 14:50_

I think I got the dangling comments working, besides the ones interspersed with parameters. Those seem a bit trickier because they change whether the lambda needs to be parenthesized. I'm running into an instability with my first naive pass, but I'll keep looking. It was pretty nice to bail out of the parameter flattening if there were any comments :laughing: but I agree that this would look better if we can get it to work.

---

_@MichaReiser reviewed on 2025-12-05 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:456 on 2025-12-05 15:08_

>  It was pretty nice to bail out of the parameter flattening if there were any comments 

We can still bail if there are any comments within `Parameters`. I just think we shouldn't if there are comments after the `lambda`, before the body

---

_@MichaReiser reviewed on 2025-12-05 15:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 15:27_

You only want to add `(` if the expression breaks (which I think isn't necessarily the case if you only have dangling end of line comments). If the expression is guaranteed to always break, use `block_indent` over `soft_block_indent`

---

_@ntBre reviewed on 2025-12-05 15:30_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 15:30_

Don't we need a line break after the dangling comment, which will always make the expression break? I might be thinking about this the wrong way.

---

_@MichaReiser reviewed on 2025-12-05 16:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 16:39_

Not necessarily: It could get flattened to:

```py
# input 
a = (
	lambda # comment
	x: body
)

a = lambda x: body # comment
```

The formatter doesn't immediately flush end-of-line comments. Instead, they're queued up and only rendered when the formatter hits a line break. You can force the formatter to emit comments sooner by writing a `line_suffix_boundary` (which can be useful if you don't want comments from wandering off)

Now, whether this is the desired formatting is a separate discussion (I think it makes sense to flatten if we can)

---

_@ntBre reviewed on 2025-12-05 16:41_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 16:41_

Oh boy, okay. I guess I have to figure out this instability since that changes whether the whole thing is parenthesized too.

---

_@ntBre reviewed on 2025-12-05 16:49_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 16:49_

Oh your example is still between the lambda and its parameters, so that's the same instability I mentioned above. I've only handled cases like this so far:

```py
(
     lambda name: # dangling header comment    
    (
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)

# Format to, includding call and subscript expressions
(
     lambda name: ( # dangling header comment    
        source_bucket
        if name == source_bucket_name
        else storage.Bucket(mock_service, destination_bucket_name)
    )
)
```

Where the dangling comments are _after_ the parameters, and we want to move them into the parenthesized body.

---

_@ntBre reviewed on 2025-12-05 21:44_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-05 21:44_

I think if we want to move any comments before parameters, we would also have to move parameter comments. There's a tricky preexisting test case:

```py
(
    lambda # comment 1
    * # comment 2
    x: # comment 3
    x
)
```

`comment 2` is initially a parameter comment, but our [stable](https://play.ruff.rs/71a14e0e-4202-4caf-910c-c4c42c58078b) formatting moves it to be a dangling lambda comment:

```py
(
    lambda  # comment 1
    # comment 2
    *x:  # comment 3
    x
)
```

This is causing trouble in our partitioning logic because I was trying to restrict our dangling comment formatting to cases without comments in the parameters, but the initial check for comments in parameters is true, so we leave `comment 2` alone, but on a second formatting, "comments in parameters" is false, and we try to format it with the dangling logic.

We have a similar problem if we don't check for comments in parameters and move the dangling comments unconditionally. On the first format, we change the order, and then the second format still tries to move comment 2 again:

```diff
 (                            
-    lambda # comment 2       
-    *x: (                    
+    lambda *x: (  # comment 2
         # comment 1          
         # comment 3          
         x                    
```

I got the other comment instabilities worked out, though, so I can take another look at this too.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-08 18:22_

Alright, I may have gone overboard on moving comments around, but I got something working. We now format the case above like this:

```py
(
    lambda *x: (  # comment 1  # comment 2  # comment 3
        x
    )
)
```

instead of our stable style:

```py
(
    lambda  # comment 1
    *x:  # comment 2
    # comment 3
    x
)
```

and closer to Black's style:

```py
(lambda *x: x)  # comment 1  # comment 2  # comment 3
```

I think I've resolved all of the other comments, this was just working on our dangling comment formatting, which we also discussed in [this thread](https://github.com/astral-sh/ruff/pull/21385#discussion_r2586735153). I tried to mark most of the older comments as resolved to clean up the discussion somewhat.

I think there's a small change to our stable formatting of lambda header comments in e8540d9b08, but I didn't see any other `preview` checks in the comment placement code. I'm happy to look further into that, but it also seems like a pretty niche case and won't affect any code that we've formatted previously because our stable formatting already converts the affected comment kind into a dangling comment, as I noted above. There are also no ecosystem changes as a result of the dangling comment commits.

---

_@ntBre reviewed on 2025-12-08 18:23_

---

_Comment by @ntBre on 2025-12-09 00:16_

I guess I'll mark this ready for review again. As noted above (https://github.com/astral-sh/ruff/pull/21385#discussion_r2599665225) I'm not super confident in the new comment changes, but I think the previous round of feedback has been resolved and that this could use another look.

---

_Marked ready for review by @ntBre on 2025-12-09 00:16_

---

_@MichaReiser reviewed on 2025-12-09 10:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:153 on 2025-12-09 10:50_

It seems unfortunate that this two branches are now the same except for what the dangling comments are. I think we should create a `FormatBody` struct that implements `Format` that we can use in both branches

---

_@MichaReiser reviewed on 2025-12-09 11:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-09 11:11_

Hmm, yeah this goes too far. As explained on previous PRs, we try to always preserve the comment position: Comments that are end-of-line comments should remain end-of-line comments and own-line comments should remain own-line comments. Your formatting violates this principle. 


This diff preserves our existing formatting closer but it also requires your changes in `placement.rs`


```diff
use ruff_formatter::{FormatRuleWithOptions, RemoveSoftLinesBuffer, format_args, write};
use ruff_python_ast::{AnyNodeRef, Expr, ExprLambda};
use ruff_text_size::Ranged;
use ruff_python_trivia::SimpleTokenizer;
use ruff_text_size::{Ranged, TextRange};

use crate::builders::parenthesize_if_expands;
use crate::comments::{dangling_comments, leading_comments, trailing_comments};
use crate::expression::parentheses::{NeedsParentheses, OptionalParentheses, Parentheses};
use crate::expression::parentheses::{
    NeedsParentheses, OptionalParentheses, Parentheses, is_expression_parenthesized,
};
use crate::expression::{CallChainLayout, has_own_parentheses};
use crate::other::parameters::ParametersParentheses;
use crate::prelude::*;
        } = item;

        let body = &**body;
        let parameters = parameters.as_deref();

        let comments = f.context().comments().clone();
        let dangling = comments.dangling(item);
        let preview = is_parenthesize_lambda_bodies_enabled(f.context());

        if let Some(parameters) = parameters {
            let parameters_have_comments = comments.contains_comments(parameters.as_ref().into());
            let parameters_have_comments = comments.contains_comments(parameters.into());

            // In this context, a dangling comment can either be a comment between the `lambda` and the
            // parameters, or a comment between the parameters and the body.
            let (dangling_before_parameters, dangling_after_parameters) =
                // To prevent an instability in cases like:
                //
                // ```py
                // (
                //     lambda # comment 1
                //     * # comment 2
                //     x: # comment 3
                //     x
                // )
                // ```
                //
                // `# comment 1` and `# comment 2` also become dangling comments on the lambda, so
                // in preview, we include these in `dangling_after_parameters`, as long as the
                // parameter list doesn't include any additional comments.
                //
                // This ends up formatted as:
                //
                // ```py
                // (
                //     lambda *x: (  # comment 1  # comment 2  # comment 3
                //         x
                //     )
                // )
                // ```
                //
                // instead of the stable formatting:
                //
                // ```py
                // (
                //     lambda  # comment 1
                //     *x:  # comment 2
                //     # comment 3
                //     x
                // )
                // ```
                if preview && !parameters_have_comments {
                    ([].as_slice(), dangling)
                } else {
                    dangling.split_at(
                        dangling.partition_point(|comment| comment.end() < parameters.start()),
                    )
                };
            let (dangling_before_parameters, dangling_after_parameters) = dangling
                .split_at(dangling.partition_point(|comment| comment.end() < parameters.start()));

            let (end_of_line_lambda_keyword_comments, leading_parameter_comments) = if preview {
                dangling_before_parameters.split_at(
                    dangling_before_parameters
                        .iter()
                        .position(|comment| comment.line_position().is_own_line())
                        .unwrap_or(dangling_before_parameters.len()),
                )
            } else {
                ([].as_slice(), dangling_before_parameters)
            };

            // To prevent an instability in cases like:
            //
            // ```py
            // (
            //     lambda # comment 1
            //     * # comment 2
            //     x: # comment 3
            //     x
            // )
            // ```
            //
            // `# comment 1` and `# comment 2` also become dangling comments on the lambda, so
            // in preview, we include these in `dangling_after_parameters`, as long as the
            // parameter list doesn't include any additional comments.
            //
            // This ends up formatted as:
            //
            // ```py
            // (
            //     lambda *x: (  # comment 1  # comment 2  # comment 3
            //         x
            //     )
            // )
            // ```
            //
            // instead of the stable formatting:
            //
            // ```py
            // (
            //     lambda  # comment 1
            //     *x:  # comment 2
            //     # comment 3
            //     x
            // )
            // ```

            trailing_comments(end_of_line_lambda_keyword_comments).fmt(f)?;

            if dangling_before_parameters.is_empty() {
            if leading_parameter_comments.is_empty() && !comments.has_leading(parameters) {
                write!(f, [space()])?;
            } else {
                write!(f, [dangling_comments(dangling_before_parameters)])?;
                write!(
                    f,
                    [
                        hard_line_break(),
                        leading_comments(leading_parameter_comments)
                    ]
                )?;
            }

            // Try to keep the parameters on a single line, unless there are intervening comments.
                //
                // and alternate between own line and end of line.
                let (dangling_end_of_line, dangling_own_line) = dangling_after_parameters.split_at(
                    dangling_after_parameters
                        .iter()
                        .position(|comment| comment.line_position().is_own_line())
                        .unwrap_or(dangling_after_parameters.len()),
                );
                let (after_parameters_end_of_line, leading_body_comments) =
                    dangling_after_parameters.split_at(
                        dangling_after_parameters
                            .iter()
                            .position(|comment| comment.line_position().is_own_line())
                            .unwrap_or(dangling_after_parameters.len()),
                    );

                let fmt_body = format_with(|f: &mut PyFormatter| {
                    write!(
                            space(),
                            token("("),
                            trailing_comments(dangling_end_of_line),
                            trailing_comments(after_parameters_end_of_line),
                            block_indent(&format_args!(
                                leading_comments(dangling_own_line),
                                leading_comments(leading_body_comments),
                                body.format().with_options(Parentheses::Never)
                            )),
                            token(")")

        if preview {
            let body_comments = comments.leading_dangling_trailing(&**body);
            let body_comments = comments.leading_dangling_trailing(body);
            let fmt_body = format_with(|f: &mut PyFormatter| {
                // If the body has comments, we always want to preserve the parentheses. This also
                // ensures that we correctly handle parenthesized comments, and don't need to worry
                // )
                // ```
                else if matches!(&**body, Expr::Call(_) | Expr::Subscript(_)) {
                else if matches!(body, Expr::Call(_) | Expr::Subscript(_)) {
                    let unparenthesized = body.format().with_options(Parentheses::Never);
                    if CallChainLayout::from_expression(
                        body.into(),

(
    lambda  # comment 1
    lambda
    # comment 1
    *x:  # comment 2
    # comment 3
    x

(
    lambda  # 1
    lambda
    # 1
    # 2
    x:  # 3
    # 4

(
    lambda  # 1
    lambda
    # 1
    # 2
    x,  # 3
    # 4

(
    lambda  # comment
    lambda
    # comment
    *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs)
    + 1
)

(
    lambda  # comment 1
    lambda
    # comment 1
    # comment 2
    *args, **kwargs:  # comment 3
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1

(
    lambda  # comment 1
    lambda
    # comment 1
    *args, **kwargs:  # comment 3
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
)

(
    lambda  # 1
    lambda
    # 1
    # 2
    left,  # 3
    # 4
     pass
 
@@ -102,58 +83,63 @@
@@ -102,22 +83,25 @@
 
 # Dangling comments without parameters.
 (
 
 (
-    lambda
-    # comment
-    *x: x
+    lambda *x: (
+        # comment
+        x
+    )
 )
 
 (
-    lambda
-    # comment
-    *x, **y: x
+    lambda *x, **y: (
+        # comment
+        x
+    )
 )
 
@@ -135,18 +119,17 @@
 (
-    lambda
-    # comment 1
     lambda
     # comment 1
-    *x:
-    # comment 2
-    # comment 3
-    x
+    lambda *x: (
+        # comment 1
+    *x: (
+        # comment 2
+        # comment 3
+        x
 
 (
-    lambda  # comment 1
-    lambda
-    # comment 1
-    *x:  # comment 2
-    # comment 3
-    x
 
 lambda *x: x
 
 (
-    lambda
-    # comment
-    *x: x
+    lambda *x: (
+        # comment
+        x
+    )
 )
 
 lambda: (  # comment
@@ -161,42 +147,47 @@
@@ -162,54 +145,58 @@
 )
 
 (
 
 (
-    lambda  # 1
-    # 2
-    lambda
-    # 1
+    lambda  # 1
     # 2
-    x:  # 3
-    # 4
-    # 5
-    # 6
-    x
+    lambda x: (  # 1
+        # 2
+        # 3
+    x: (  # 3
+        # 4
+        # 5
+        # 6
 
 (
@@ -204,9 +195,10 @@
-    lambda
-    # 1
+    lambda  # 1
     # 2
     x,  # 3
     # 4
 
 (
@@ -218,71 +210,79 @@
@@ -221,71 +208,79 @@
 
 # Leading
 lambda x: (
 
 # Regression tests for https://github.com/astral-sh/ruff/issues/8179
@@ -291,9 +291,9 @@
@@ -294,9 +289,9 @@
         c,
         d,
         e,
 
 
@@ -302,15 +302,9 @@
@@ -305,15 +300,9 @@
         c,
         d,
         e,
     )
 
@@ -320,9 +314,9 @@
@@ -323,9 +312,9 @@
         c,
         d,
         e,
 
 
@@ -338,9 +332,9 @@
@@ -341,9 +330,9 @@
 
 class C:
     function_dict: Dict[Text, Callable[[CRFToken], Any]] = {
 
 
@@ -352,42 +346,40 @@
@@ -355,42 +344,40 @@
 def foo():
     if True:
         if True:
         SELECT ROW_NUMBER() OVER () AS id, {var}
         FROM (
@@ -402,18 +394,19 @@
@@ -405,18 +392,19 @@
 long_assignment_target.with_attribute.and_a_slice[with_an_index] = (
     # 1
     # 2
 
 very_long_variable_name_x, very_long_variable_name_y = (
@@ -421,8 +414,8 @@
@@ -424,8 +412,8 @@
     lambda b: b * another_very_long_expression_here,
 )
 
     )
 )
@@ -458,12 +451,12 @@
@@ -461,12 +449,12 @@
     [
         # Not fluent
         param(
         param(
             lambda left, right: (
@@ -472,9 +465,9 @@
@@ -475,9 +463,9 @@
         ),
         param(lambda left, right: ibis.timestamp("2017-04-01").cast(dt.date)),
         param(
         # This is too long on one line in the lambda body and gets wrapped
         # inside the body.
@@ -508,16 +501,18 @@
@@ -511,16 +499,18 @@
 ]
 
 # adds parentheses around the body
 lambda x, y, z: (
     x + y + z
@@ -528,7 +523,7 @@
@@ -531,7 +521,7 @@
     x + y + z  # trailing eol body
 )
 
 lambda x, y, z: (
     # leading body
@@ -540,21 +535,23 @@
@@ -543,21 +533,23 @@
 )
 
 (
     source_bucket
     if name == source_bucket_name
@@ -562,8 +559,7 @@
@@ -565,8 +557,7 @@
 )
 
 (
         if name == source_bucket_name
         else storage.Bucket(mock_service, destination_bucket_name)
@@ -571,61 +567,70 @@
@@ -574,61 +565,70 @@
 )
 
 (
 
 (
@@ -636,29 +641,30 @@
 )
 
@@ -641,51 +641,50 @@
 (
-    lambda
-    # comment
     lambda
     # comment
-    *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs)
-    + 1
+    lambda *args, **kwargs: (
+        # comment
+    *args, **kwargs: (
+        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
+    )
 )
 
 (
-    lambda  # comment
-    lambda
-    # comment
-    *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs)
-    + 1
+    lambda *args, **kwargs: (  # comment
 
 (
-    lambda  # comment 1
-    # comment 2
-    lambda
-    # comment 1
+    lambda  # comment 1
     # comment 2
-    *args, **kwargs:  # comment 3
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
+    lambda *args, **kwargs: (  # comment 1
+        # comment 2
+        # comment 3
+    *args, **kwargs: (  # comment 3
+        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
+    )
 )
 
 (
-    lambda  # comment 1
-    lambda
-    # comment 1
-    *args, **kwargs:  # comment 3
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs) + 1
+    lambda *args, **kwargs: (  # comment 1  # comment 3
 
 (
@@ -666,19 +672,20 @@
-    lambda
-    # 1
+    lambda  # 1
     # 2
     left,  # 3
     # 4
     )
 )
@@ -696,46 +703,50 @@
@@ -703,46 +702,50 @@
 foo(
     lambda from_ts,  # but still wrap the body if it gets too long
     to_ts,
diff --git a/crates/ruff_python_formatter/src/expression/expr_lambda.rs b/crates/ruff_python_formatter/src/expression/expr_lambda.rs
index 18ab36997b..082e694d8f 100644
--- a/crates/ruff_python_formatter/src/expression/expr_lambda.rs
+++ b/crates/ruff_python_formatter/src/expression/expr_lambda.rs
@@ -1,10 +1,13 @@
 use ruff_formatter::{FormatRuleWithOptions, RemoveSoftLinesBuffer, format_args, write};
 use ruff_python_ast::{AnyNodeRef, Expr, ExprLambda};
-use ruff_text_size::Ranged;
+use ruff_python_trivia::SimpleTokenizer;
+use ruff_text_size::{Ranged, TextRange};
 
 use crate::builders::parenthesize_if_expands;
 use crate::comments::{dangling_comments, leading_comments, trailing_comments};
-use crate::expression::parentheses::{NeedsParentheses, OptionalParentheses, Parentheses};
+use crate::expression::parentheses::{
+    NeedsParentheses, OptionalParentheses, Parentheses, is_expression_parenthesized,
+};
 use crate::expression::{CallChainLayout, has_own_parentheses};
 use crate::other::parameters::ParametersParentheses;
 use crate::prelude::*;
@@ -24,6 +27,9 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
             body,
         } = item;
 
+        let body = &**body;
+        let parameters = parameters.as_deref();
+
         let comments = f.context().comments().clone();
         let dangling = comments.dangling(item);
         let preview = is_parenthesize_lambda_bodies_enabled(f.context());
@@ -31,58 +37,72 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
         write!(f, [token("lambda")])?;
 
         if let Some(parameters) = parameters {
-            let parameters_have_comments = comments.contains_comments(parameters.as_ref().into());
+            let parameters_have_comments = comments.contains_comments(parameters.into());
 
             // In this context, a dangling comment can either be a comment between the `lambda` and the
             // parameters, or a comment between the parameters and the body.
-            let (dangling_before_parameters, dangling_after_parameters) =
-                // To prevent an instability in cases like:
-                //
-                // ```py
-                // (
-                //     lambda # comment 1
-                //     * # comment 2
-                //     x: # comment 3
-                //     x
-                // )
-                // ```
-                //
-                // `# comment 1` and `# comment 2` also become dangling comments on the lambda, so
-                // in preview, we include these in `dangling_after_parameters`, as long as the
-                // parameter list doesn't include any additional comments.
-                //
-                // This ends up formatted as:
-                //
-                // ```py
-                // (
-                //     lambda *x: (  # comment 1  # comment 2  # comment 3
-                //         x
-                //     )
-                // )
-                // ```
-                //
-                // instead of the stable formatting:
-                //
-                // ```py
-                // (
-                //     lambda  # comment 1
-                //     *x:  # comment 2
-                //     # comment 3
-                //     x
-                // )
-                // ```
-                if preview && !parameters_have_comments {
-                    ([].as_slice(), dangling)
-                } else {
-                    dangling.split_at(
-                        dangling.partition_point(|comment| comment.end() < parameters.start()),
-                    )
-                };
+            let (dangling_before_parameters, dangling_after_parameters) = dangling
+                .split_at(dangling.partition_point(|comment| comment.end() < parameters.start()));
+
+            let (end_of_line_lambda_keyword_comments, leading_parameter_comments) = if preview {
+                dangling_before_parameters.split_at(
+                    dangling_before_parameters
+                        .iter()
+                        .position(|comment| comment.line_position().is_own_line())
+                        .unwrap_or(dangling_before_parameters.len()),
+                )
+            } else {
+                ([].as_slice(), dangling_before_parameters)
+            };
+
+            // To prevent an instability in cases like:
+            //
+            // ```py
+            // (
+            //     lambda # comment 1
+            //     * # comment 2
+            //     x: # comment 3
+            //     x
+            // )
+            // ```
+            //
+            // `# comment 1` and `# comment 2` also become dangling comments on the lambda, so
+            // in preview, we include these in `dangling_after_parameters`, as long as the
+            // parameter list doesn't include any additional comments.
+            //
+            // This ends up formatted as:
+            //
+            // ```py
+            // (
+            //     lambda *x: (  # comment 1  # comment 2  # comment 3
+            //         x
+            //     )
+            // )
+            // ```
+            //
+            // instead of the stable formatting:
+            //
+            // ```py
+            // (
+            //     lambda  # comment 1
+            //     *x:  # comment 2
+            //     # comment 3
+            //     x
+            // )
+            // ```
+
+            trailing_comments(end_of_line_lambda_keyword_comments).fmt(f)?;
 
-            if dangling_before_parameters.is_empty() {
+            if leading_parameter_comments.is_empty() && !comments.has_leading(parameters) {
                 write!(f, [space()])?;
             } else {
-                write!(f, [dangling_comments(dangling_before_parameters)])?;
+                write!(
+                    f,
+                    [
+                        hard_line_break(),
+                        leading_comments(leading_parameter_comments)
+                    ]
+                )?;
             }
 
             // Try to keep the parameters on a single line, unless there are intervening comments.
@@ -124,12 +144,13 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
                 // ```
                 //
                 // and alternate between own line and end of line.
-                let (dangling_end_of_line, dangling_own_line) = dangling_after_parameters.split_at(
-                    dangling_after_parameters
-                        .iter()
-                        .position(|comment| comment.line_position().is_own_line())
-                        .unwrap_or(dangling_after_parameters.len()),
-                );
+                let (after_parameters_end_of_line, leading_body_comments) =
+                    dangling_after_parameters.split_at(
+                        dangling_after_parameters
+                            .iter()
+                            .position(|comment| comment.line_position().is_own_line())
+                            .unwrap_or(dangling_after_parameters.len()),
+                    );
 
                 let fmt_body = format_with(|f: &mut PyFormatter| {
                     write!(
@@ -137,9 +158,9 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
                         [
                             space(),
                             token("("),
-                            trailing_comments(dangling_end_of_line),
+                            trailing_comments(after_parameters_end_of_line),
                             block_indent(&format_args!(
-                                leading_comments(dangling_own_line),
+                                leading_comments(leading_body_comments),
                                 body.format().with_options(Parentheses::Never)
                             )),
                             token(")")
@@ -196,7 +217,7 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
         }
 
         if preview {
-            let body_comments = comments.leading_dangling_trailing(&**body);
+            let body_comments = comments.leading_dangling_trailing(body);
             let fmt_body = format_with(|f: &mut PyFormatter| {
                 // If the body has comments, we always want to preserve the parentheses. This also
                 // ensures that we correctly handle parenthesized comments, and don't need to worry
@@ -232,7 +253,7 @@ impl FormatNodeRule<ExprLambda> for FormatExprLambda {
                 //     ),
                 // )
                 // ```
-                else if matches!(&**body, Expr::Call(_) | Expr::Subscript(_)) {
+                else if matches!(body, Expr::Call(_) | Expr::Subscript(_)) {
                     let unparenthesized = body.format().with_options(Parentheses::Never);
                     if CallChainLayout::from_expression(
                         body.into(),
```

However, I think we can simplify the implementation if we move more of the logic into `placement.rs`:

* Pass `preview_mode` to `place_comment`. 
* Can we change `place_comment` to always make the `leading_body_comments`/`dangling_own_line` leading comments of the body? I think we should then get the entire `fmt_body` for free

---

_@ntBre reviewed on 2025-12-09 13:45_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-09 13:45_

> As explained on previous PRs, we try to always preserve the comment position: Comments that are end-of-line comments should remain end-of-line comments and own-line comments should remain own-line comments. Your formatting violates this principle.

Oh, I thought I had preserved that, at least in this case. All three comments here start out as end-of-line comments and remain that way in my preview format, but maybe you're talking about a different example. Actually our stable formatting changes the placement in this case because comment 2 becomes an own-line comment.

```py
# input
(
    lambda # comment 1
    * # comment 2
    x: # comment 3
    x
)

# stable: https://play.ruff.rs/a0980704-a9a0-4c7f-83e5-0f18bed89f72?secondary=Format
(
    lambda  # comment 1
    # comment 2
    *x:  # comment 3
    x
)

# preview
(
    lambda *x: (  # comment 1  # comment 2  # comment 3
        x
    )
)
```

Anyway, thanks for the patch. I'll apply that and then look into moving more of the logic into `placement.rs`.

---

_@ntBre reviewed on 2025-12-09 23:30_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__lambda.py.snap`:2004 on 2025-12-09 23:30_

This is not right. The first comment should stay on the first line, like in the [playground](https://play.ruff.rs/326510c9-8457-408c-90f6-b6bb3d1fd060?secondary=Format) and on main.

I tracked it down to the assignment statement changes on this branch, but I still need to resolve it. I think the rest of the output is looking as I want, though.

I don't want GitHub to discard any of the old review comments, but I can also squash this down heavily if it would help with review. I think it would be nice to see all of the test snapshots before any of the code changes to make sure the stable formatting doesn't change accidentally. I could even spin the tests off into a separate PR, if that would help.

---

_Converted to draft by @ntBre on 2025-12-10 00:07_

---

_@MichaReiser reviewed on 2025-12-10 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:312 on 2025-12-10 07:15_

Can we gate the `if let Expr::Lambda` with the preview check to avoid this branch on stable (and remove the need for this comment)?

For preserving the `( # comment`, you want a branch similar to this in `maybe_parenthesize_expression`:

```rust
        // If the expression has comments, we always want to preserve the parentheses. This also
        // ensures that we correctly handle parenthesized comments, and don't need to worry about
        // them in the implementation below.
        if node_comments.has_leading() || node_comments.has_trailing_own_line() {
            return expression.format().with_options(Parentheses::Always).fmt(f);
        }
```

In your case, it's probably sufficient to just check if the lambda has any leading comments and, if so, don't take this branch

---

_@ntBre reviewed on 2025-12-10 14:04_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:312 on 2025-12-10 14:04_

Ah yeah, that's much better. Thanks!

---

_@ntBre reviewed on 2025-12-10 15:01_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-10 15:01_

I think I got this working in `placement.rs` while mostly deleting both of the duplicated `fmt_body` blocks. I still had to preserve some of the logic in the `body_comments.has_leading()` block, so I may have missed something, though. 

Only one snapshot changed, but I think maybe it's more correct now since we preserve the line placement, as long as it's okay to stack comments?

```py
(
    lambda:  # comment 1
    (  # comment 2
        x
    )
)
```

now formats as

```py
(
    lambda: (  # comment 1  # comment 2
        x
    )
)
```

instead of

```py
(
    lambda: (  # comment 1
        # comment 2
        x
    )
)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1888 on 2025-12-10 16:55_

Ah, hmm. This will be a footgun to stabilize because it doesn't use our normal function to check whether the feature is on. maybe add an extra function that takes the `PreviewMode` instead of the `FormatContext` so that we can use it here too

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1 on 2025-12-10 16:57_

Let's update the comments on the function you've changed and document the preview behavior. It's very unlikely that the person promoting the style would update the documentation.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:101 on 2025-12-10 16:59_

Cool, how small the comment placement change is now :)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-10 17:00_

Poking into the formatting of inner expressions is something we should avoid whenever we can because it erodes isolation. 


Can't we call `body.format` directly (may require `Parentheses::Always`) here? I think it should format the comments exactly the way you want (and add parentheses too) 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1888 on 2025-12-10 17:02_

Do we even need the `SimpleTokenizer` call if preview is enabled?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1922 on 2025-12-10 17:06_

I'm not convinced we should do this for the example in the comment (right after the `else`) because it doesn't match my mental model of what's considered leading comments: An end-of-line comment is a trailing comment. 

It is nice that it simplifies the formatting logic but I would rather do a little more in the lambda formatting than be surprised in the formatting of any given expression that it now suddenly has an end-of-line comment as its leading comment

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-10 17:06_

Are these now always own line comments? If so, can we use `leading_comments` with a `hard_line_break` instead?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:1368 on 2025-12-10 17:09_

I think the name here is misleading. It suggests that it only applies to lambda expressions. `maybe_parenthesize_value`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:630 on 2025-12-10 17:11_

Can you add some tests where the call expression is or isn't parenthesized and has comments?

---

_@MichaReiser reviewed on 2025-12-10 17:13_

We're very close. My only real concern here is that I don't think we should make end-of-line comments leading comments (assuming I understand the change correctly).


I've to head out soon, so I won't be able to review the ecosystem changes.

---

_@ntBre reviewed on 2025-12-10 17:22_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1888 on 2025-12-10 17:22_

Oh yeah, looks like I can move the token check into the non-preview branch. And I added another preview function, good call!

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1 on 2025-12-10 18:25_

Oh good catch. I'm attempting to put a `preview` docs link into the comment so that the doc tests will fail when the preview function is removed.

---

_@ntBre reviewed on 2025-12-10 18:25_

---

_@ntBre reviewed on 2025-12-10 18:27_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-10 18:27_

`body.format().with_options(Parentheses::Always)` is all we had here before, but it was placing the leading comments before the parentheses of the body, as in:

```py
(
    lambda:  # leading comment 1
    (  # comment 2
        x
    )
)
```

instead of:

```py
(
    lambda: (  # leading comment 1  # comment 2
        x
    )
)
```

or similar. This is where I suspected I was missing something in my [comment](https://github.com/astral-sh/ruff/pull/21385#discussion_r2607024760) above. Maybe we actually prefer the first result? That's what we do on stable.

If not, I'll take another look at this after the other comments!

---

_@ntBre reviewed on 2025-12-10 18:42_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-10 18:42_

I believe these are actually always end-of-line comments. This is part of the updated docs I added to `handle_lambda_comment`:

```rust
/// In [preview](is_parenthesize_lambda_bodies_enabled_preview), the comment placement is instead:
///
/// ```python
/// (
///     lambda  # dangling lambda
///     # leading parameters
///     x
///     :  # leading body
///     # leading body
///     y
/// )
/// ```
```

and a temporary assertion here confirms that:

```rust
                assert!(
                    dangling_before_parameters
                        .iter()
                        .all(|comment| comment.line_position().is_end_of_line())
                );
```

at least on our tests.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1922 on 2025-12-10 19:09_

Ah yeah, that makes sense to me. So should I just revert this and go back to handling it in the lambda formatting? I can just factor out the shared code like you mentioned before.

---

_@ntBre reviewed on 2025-12-10 19:09_

---

_@ntBre reviewed on 2025-12-10 21:05_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1922 on 2025-12-10 21:05_

I reverted the preview placement changes and factored out `FormatBody`, assuming that's what you meant. I think that resolves this comment and the one above (https://github.com/astral-sh/ruff/pull/21385#discussion_r2607482398).

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/placement.rs`:1 on 2025-12-10 21:05_

(opened https://github.com/astral-sh/ruff/pull/21903 to help enforce the links)

---

_@ntBre reviewed on 2025-12-10 21:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-11 07:57_

I would have to play with the most recent version to say why this is happening but I would expect this to get formatted like your second code snippet when using `trailing_comments` for `leading comment 1`. 

But that also assumes we place the comments correctly:

```py
(
	lambda: # dangling-end-of-line
	# leading-body-own-line
	(  # leading-body-end-of-line (okay, we sometimes have end of line comments :))
		x
	)
)
```

I'd expect the following IR to work

```
if has_comments... {
	write!(f, [
		trailing_comments(dangling-end-of-line),
		body.format().with_options(Parentheses::Always),
	])?;
} else {
	...
}
```

It may be necessary to insert a `hard_line_break` if `body` has a leading own-line comment but not 100% sure about that:

It should format my example to 

```py
(
	lambda: # dangling-end-of-line
	# leading-body-own-line
	(  # leading-body-end-of-line (okay, we sometimes have end of line comments :))
		x
	)
)
```

But the `# dangling-end-of-line` should float to the back after the `(` if there's no `leading-body-own-line` comment like so

```py
(
	lambda: (  # dangling-end-of-line # leading-body-end-of-line (okay, we sometimes have end of line comments :))
		x
	)
)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 07:59_

I would then recommend using `trailing_comments` and ideally change the variable naming too

---

_@MichaReiser reviewed on 2025-12-11 08:00_

---

_@ntBre reviewed on 2025-12-11 15:09_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-11 15:09_

Your `leading-body-own-line` comment is currently placed as a dangling comment on the lambda, I guess that's why your snippet is formatted like this in preview:

```py
(
    lambda: (  # dangling-end-of-line
        # leading-body-own-line
        # leading-body-end-of-line (okay, we sometimes have end of line comments :))
        x
    )
)
```

Your output is the stable formatting, though.

I'll start playing with making `leading-body-own-line` a leading comment on the body in a separate branch, it's a bit hard to isolate all the variables here with the preview changes mixed in.

---

_@ntBre reviewed on 2025-12-11 16:45_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-11 16:45_

Okay I got something working by just manipulating the dangling comments a bit more. I don't think we want to make `leading-body-own-line` a leading comment because it complicates a lot of other situations like these:

```py
(
    lambda
    # 3
    : None
)

(
    lambda  # 1
    # 2
    : # 3
    # 4
    None # 5
)

a = (
    lambda  # Dangling
           : 1
)

a = (
    lambda:  # Dangling
    1
)
```

I saved some of my experiments trying to get these to work in 2cb98d4cdb9 before reverting it and trying a different approach in `FormatBody`.

---

_@MichaReiser reviewed on 2025-12-11 16:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:169 on 2025-12-11 16:52_

> Okay I got something working by just manipulating the dangling comments a bit more. I don't think we want to make leading-body-own-line a leading comment because it complicates a lot of other situations like these:

We don't have to if we can find another solution and I'm fine with that. But I don't follow how or why it complicates the examples you shared. Again, no reason to explain if you have a working solution.

---

_@ntBre reviewed on 2025-12-11 17:02_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 17:02_

I think using `trailing_comments` here conflicts with the `trailing_comments` changes I just pushed for the body. Comments from before the parameters are migrating all the way into the body, and in the wrong order, at least when simply swapping `dangling_comments` for `trailing_comments` (it's also unstable).

```py
# input
(
    lambda # comment 1
    * # comment 2
    x: # comment 3
    x
)

# first output
(
    lambda # comment 2  # comment 1
    *x:  # comment 3
    x
)

# second output
(
    lambda *x:  # comment 3  # comment 2  # comment 1
    x
)
```

(This suggestion may be outdated after some of the other changes, but I did give it a try)

---

_Marked ready for review by @ntBre on 2025-12-11 17:11_

---

_@MichaReiser reviewed on 2025-12-11 17:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 17:24_

The order shouldn't be changed unless this is a bug in the printer for as long as they appear in the right order in the IR. But again, hard to say without debugging this msyelf

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:411 on 2025-12-11 17:25_

You use `f.context().comments()` a lot. I think it makes sense to assign it to a variable.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:432 on 2025-12-11 17:27_

I like to move the common code out of the conditions

```rust
trailing_comments(dangling.fmt())?;

if leading_body_comments.is_empty() {
	space.fmt(f)?;
} else {
	hard_line_break().fmt(f)?;
}

body.format().with_options(Parentheses::Always).fmt(f)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:414 on 2025-12-11 17:28_

Why don't we need to format the `leading_body_comments` if they aren't empty as we do in the `else` branch

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 17:28_

I don't want to belabor the point if we're happy with the current version, but this is the IR, if you're curious:

```
cargo run --bin ruff_python_formatter -- --emit stdout --target-version 3.10 --print-ir
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff_python_formatter --emit stdout --target-version 3.10 --print-ir`
[
  "(",
  group(expand: propagated, [
    indent([
      soft_line_break,
      "lambda ",
      line_suffix(13, ["  # comment 1"]),
      expand_parent,
      "# comment 2",
      hard_line_break,
      group(["*x"]),
      ":  # comment 3",
      hard_line_break,
      "x"
    ]),
    soft_line_break
  ]),
  ")",
  hard_line_break
]
(
    lambda # comment 2  # comment 1
    *x:  # comment 3
    x
)
```

---

_@ntBre reviewed on 2025-12-11 17:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:344 on 2025-12-11 17:29_

Nit: For inline helpers, I tend to just call the struct initializer

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:133 on 2025-12-11 17:31_

It would be more helpful if the comment said why that's the case. I otherwise suggest removing it

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:135 on 2025-12-11 17:32_

I think `format_body` should be renamed to `parenthesized_body` or `body_with_comments` or what not if this really is about formatting a parenthesized body and not the function to format any lambda body

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 17:35_

I see. The issue is that `comment 3` doesn't use `trailing_comments`. So `comment 3` doesn't become a line suffix and, instead, gets rendered in place. 



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:148 on 2025-12-11 17:43_

Uff, I found this flow with early returns extremelly confusing. I was like, wait a minue. Aren't we formatting the body twice. 

I think I'd prefer to unify it with the `preview` branch below. I feel like this would simplify things because 

1. You don't need to repeat the logic twice. In preview, just don't format the dangling comments up here
2. The reading flow becomes a more natural top-down flow. 
3. It removes to match on the layout twice

---

_@MichaReiser reviewed on 2025-12-11 17:54_

Thank you. I think this looks good logic wise. It's great to see that this now requires way fewer changes than first expect.

It would have been nice if there were fewer places we used `dangling_comments` but I'm fine leaving this where we are now. 


I think we should try to unify the body formatting logic in preview mode to make the code easier to read. 

We should also do one more pass over the ecosystem changes.



---

_Comment by @ntBre on 2025-12-11 17:58_

Thank you! I'll go through these new comments this afternoon.

I'll also go through the ecosystem results once more. I've been keeping up with them, including a full look at a local run yesterday since the comment is truncated. I'll do that again after these code changes.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:135 on 2025-12-11 18:01_

I mark this as resolved because this will just become `format_body` if you address my other comment about unifying the body formatting

---

_@MichaReiser reviewed on 2025-12-11 18:01_

---

_@ntBre reviewed on 2025-12-11 20:13_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:432 on 2025-12-11 20:13_

Oh nice, thanks! I thought the space had to go before `trailing_comments` in the first arm, but this works perfectly.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:414 on 2025-12-11 20:15_

We're formatting `dangling` as a whole here, so we don't need to format `after_parameters_end_of_line` and `leading_body_comments` separately.

---

_@ntBre reviewed on 2025-12-11 20:15_

---

_@ntBre reviewed on 2025-12-11 20:18_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:133 on 2025-12-11 20:18_

Oh yeah, I think this is partially outdated and handled better by the comments in `FormatBody`, so I'll just delete it.

---

_@ntBre reviewed on 2025-12-11 20:23_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:107 on 2025-12-11 20:23_

Comments 1 and 2 still swap their order in this case, even though they seem to be in the right order in the IR, unless I'm missing something. And it happens without comment 3 too:

```
❯ cat <<EOF | just fmt --print-ir
(
    lambda # comment 1
    * # comment 2
    x:
    x
)
EOF
[
  "(",
  group(expand: propagated, [
    indent([
      soft_line_break,
      "lambda ",
      line_suffix(13, ["  # comment 1"]),
      expand_parent,
      "# comment 2",
      hard_line_break,
      group(["*x"]),
      ": x"
    ]),
    soft_line_break
  ]),
  ")",
  hard_line_break
]
(
    lambda # comment 2  # comment 1
    *x: x
)
```

---

_@ntBre reviewed on 2025-12-11 20:34_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:148 on 2025-12-11 20:34_

Ah that's much nicer. I felt that it was complicated but hadn't found a good way to fix it. Thanks!

---

_Comment by @ntBre on 2025-12-11 21:05_

I think that should take care of the code changes, just waiting for the ecosystem comment. I'll download the results, go through them, and then upload a copy here. I was getting a slightly different number of changes when running ruff-ecosystem locally, so I'll stick to the canonical one from CI.

Differently, are my comments in the code too long? It felt like a bad sign that I installed a plugin to hide comments this afternoon to get a better look at the overall code structure. Otherwise it seemed nice to make them very explicit.

---

_Comment by @ntBre on 2025-12-11 21:28_

Ecosystem results look good to me! As expected and noted in the summary, we now:
- Keep lambda parameters on one line
- Parenthesize the lambda body if it expands
- Remove parentheses around the lambda body if they're no longer needed

[ecosystem-result.md](https://github.com/user-attachments/files/24112238/ecosystem-result.md)


---

_@MichaReiser reviewed on 2025-12-12 09:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:414 on 2025-12-12 09:28_

This might be worth a comment

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:39 on 2025-12-12 09:39_

Let's move this closer to where it's used (line 110)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:135 on 2025-12-12 09:43_

Could we also move this into `FormatBody`, given that it's the same for both branches and that we moved the responsibility of formatting those comments into `FormatBody` anyway

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:38 on 2025-12-12 09:44_

Can we give this variable a more descriptive name (also in `FormatBody`). Like dangling where? And how are they different from dangling on line 33

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:215 on 2025-12-12 09:44_

Let's give this field a better name and add a document what sort of comments they are (where should they be placed. Can they be end of line comments and own line comments?)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:429 on 2025-12-12 09:53_

I'm inclined to move this out of `FormatBody` to remove the need for the `fmt_body` lambda

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:251 on 2025-12-12 09:55_

So `after_parameters_end_of_line` only contain `# 1`? The `after_parameters` part is a bit confusing because there are no parameters in the example but I think it's fine

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:347 on 2025-12-12 09:59_

Can you remind me again why we can't use this layout for call chains? 

---

_@MichaReiser approved on 2025-12-12 10:01_

This is looks great to me. My only question is if we can improve call chain formatting. The extra set of parentheses around call chains often feels unnecessary. It would be nice if we could avoid that, but I don't remember what the issue was that we faced. I don't think it's very important and also something we can iterate on later but might be worth giving a short try

This isn't a call chain, but I do find the parentheses very unnecessary.

<a href='https://github.com/qdrant/qdrant-client/blob/750d735b2917a30b0cbf468ddccbc47d12704e32/tests/congruence_tests/test_delete_points.py#L70'>tests/congruence_tests/test_delete_points.py~L70</a>
```diff
     compare_client_results(
         local_client,
         remote_client,
-        lambda c: c.query_points(
-            COLLECTION_NAME,
-            query=vector,
-            using="sparse-image",
-        ).points,
+        lambda c: (
+            c.query_points(
+                COLLECTION_NAME,
+                query=vector,
+                using="sparse-image",
+            ).points
+        ),
     )
 
     found_ids = [
```

<a href='https://github.com/zulip/zulip/blob/86cec937dd0ab004c9f2f50d1a853dd432e2c996/zerver/lib/user_groups.py#L788'>zerver/lib/user_groups.py~L788</a>
```diff
 
 def get_recursive_subgroups_union_for_groups(user_group_ids: list[int]) -> QuerySet[UserGroup]:
     cte = CTE.recursive(
-        lambda cte: UserGroup.objects.filter(id__in=user_group_ids)
-        .values(group_id=F("id"))
-        .union(
-            cte.join(NamedUserGroup, direct_supergroups=cte.col.group_id).values(group_id=F("id"))
+        lambda cte: (
+            UserGroup.objects.filter(id__in=user_group_ids)
+            .values(group_id=F("id"))
+            .union(
+                cte.join(NamedUserGroup, direct_supergroups=cte.col.group_id).values(
+                    group_id=F("id")
+                )
+            )
         )
     )
     return with_cte(cte, select=cte.join(UserGroup, id=cte.col.group_id))
```

---

_@ntBre reviewed on 2025-12-12 14:42_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:347 on 2025-12-12 14:42_

I think we discussed it looking better this way. Here's a quick example without the call chain formatting:

```diff
         param(
-            lambda left, right: (
-                ibis.timestamp("2017-04-01")
-                .cast(dt.date)
-                .between(left, right)
-                .between(left, right)
-            ),
+            lambda left, right: ibis.timestamp("2017-04-01")
+            .cast(dt.date)
+            .between(left, right)
+            .between(left, right),
         ),
     ],
```

which is like our [stable](https://play.ruff.rs/95e18d91-1d35-43a5-915b-40cf83c55991?secondary=Format) formatting.

---

_Comment by @ntBre on 2025-12-12 14:49_

To your first example, the `.points` is what causes us to wrap. Without that, we'll reuse the call parentheses:

```py
def foo():
    compare_client_results(
        local_client,
        remote_client,
        lambda c: (
            c.query_points(
                COLLECTION_NAME,
                query=vector,
                using="sparse-image",
            ).points
        ),
    )

    compare_client_results(
        local_client,
        remote_client,
        lambda c: c.query_points(
            COLLECTION_NAME,
            query=vector,
            using="sparse-image",
        ),
    )
```

So I think it's okay. I think that's preferable to our stable formatting:

```py
def foo():
    compare_client_results(
        local_client,
        remote_client,
        lambda c: c.query_points(
            COLLECTION_NAME,
            query=vector,
            using="sparse-image",
        ).points,
    )
```

Like the call chains, I think this becomes less clear especially when followed by arguments after the lambda, similar to the examples from #8179.

```py
def foo():
    compare_client_results(
        local_client,
        remote_client,
        lambda c: c.query_points(
            COLLECTION_NAME,
            query=vector,
            using="sparse-image",
        ).points,
        somewhat_ambiguous_argument=might_be_part_of_the_lambda
    )

---

_@ntBre reviewed on 2025-12-12 15:03_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:135 on 2025-12-12 15:03_

What do you think about something like this?

```rust
        write!(f, [token(":")])?;

        // In this context, a dangling comment is a comment between the `lambda` and the body.
        if dangling.is_empty() {
            write!(f, [space()])?;
        } else if !preview {
            write!(f, [dangling_comments(dangling)])?;
        }

        if !preview {
            return body.format().fmt(f);
        }

        let fmt_body = FormatBody { body, dangling };

        match self.layout {
            ExprLambdaLayout::Assignment => fits_expanded(&fmt_body).fmt(f),
            ExprLambdaLayout::Default => fmt_body.fmt(f),
        }
```

I see what you mean about putting the dangling comments into `FormatBody`, but this also seems nice because it moves both the preview and layout checks out of `FormatBody`.

---

_@MichaReiser reviewed on 2025-12-12 15:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:135 on 2025-12-12 15:11_

That looks good too. For as long as we move it into a shared code path

---

_@ntBre reviewed on 2025-12-12 16:33_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:38 on 2025-12-12 16:33_

I tried a couple of different names like `dangling_after_parameters` and `dangling_body_comments` before settling on `dangling_header_comments` and adding a couple of comments to explain what it means. Hopefully that's a bit better.

---

_@ntBre reviewed on 2025-12-12 16:39_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:251 on 2025-12-12 16:39_

Yes, that's right. I made this name `trailing_header_comments` since we format them with `trailing_comments`, and improved the docs here after verifying that the example is correct.

---

_Comment by @ntBre on 2025-12-12 16:45_

Thank you again for all of your help here! I've addressed the new comments and will plan to merge once CI passes, and I verify that the ecosystem checks haven't changed!

---

_Merged by @ntBre on 2025-12-12 17:02_

---

_Closed by @ntBre on 2025-12-12 17:02_

---

_Branch deleted on 2025-12-12 17:02_

---
