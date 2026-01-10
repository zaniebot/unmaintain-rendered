```yaml
number: 14380
title: Unused variable accessed
type: pull_request
state: closed
author: Lokejoke
labels: []
assignees: []
base: main
head: unused-variable-accessed
created_at: 2024-11-16T16:57:04Z
updated_at: 2024-11-16T17:11:36Z
url: https://github.com/astral-sh/ruff/pull/14380
synced_at: 2026-01-10T20:50:57Z
```

# Unused variable accessed

---

_Pull request opened by @Lokejoke on 2024-11-16 16:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Closed by @Lokejoke on 2024-11-16 16:57_

---

_Comment by @github-actions[bot] on 2024-11-16 17:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+253 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+170 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/api_connexion/endpoints/pool_endpoint.py#L135'>airflow/api_connexion/endpoints/pool_endpoint.py:135:9:</a> WPS121 Local variable `_patch_body` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/assets/manager.py#L282'>airflow/assets/manager.py:282:5:</a> WPS121 Local variable `_asset_manager_class` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/assets/manager.py#L287'>airflow/assets/manager.py:287:5:</a> WPS121 Local variable `_asset_manager_kwargs` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/cli/commands/connection_command.py#L152'>airflow/cli/commands/connection_command.py:152:5:</a> WPS121 Local variable `_connection_types` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/cli/commands/info_command.py#L263'>airflow/cli/commands/info_command.py:263:9:</a> WPS121 Local variable `_locale` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/cli/commands/task_command.py#L439'>airflow/cli/commands/task_command.py:439:9:</a> WPS121 Local variable `_dag` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/configuration.py#L1297'>airflow/configuration.py:1297:13:</a> WPS121 Local variable `_section` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/dag_processing/processor.py#L234'>airflow/dag_processing/processor.py:234:26:</a> WPS121 Local variable `_child_channel` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/dag_processing/processor.py#L234'>airflow/dag_processing/processor.py:234:9:</a> WPS121 Local variable `_parent_channel` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/decorators/base.py#L486'>airflow/decorators/base.py:486:9:</a> WPS121 Local variable `_MappedOperator` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/executors/executor_loader.py#L230'>airflow/executors/executor_loader.py:230:13:</a> WPS121 Local variable `_executor_name` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/io/path.py#L393'>airflow/io/path.py:393:9:</a> WPS121 Local variable `_kwargs` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/io/path.py#L407'>airflow/io/path.py:407:9:</a> WPS121 Local variable `_kwargs` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/jobs/scheduler_job_runner.py#L2269'>airflow/jobs/scheduler_job_runner.py:2269:9:</a> WPS121 Local variable `_executor_to_tis` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/lineage/__init__.py#L138'>airflow/lineage/__init__.py:138:17:</a> WPS121 Local variable `_inlets` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/lineage/__init__.py#L73'>airflow/lineage/__init__.py:73:5:</a> WPS121 Local variable `_backend` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/lineage/hook.py#L230'>airflow/lineage/hook.py:230:13:</a> WPS121 Local variable `_hook_lineage_collector` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/lineage/hook.py#L232'>airflow/lineage/hook.py:232:13:</a> WPS121 Local variable `_hook_lineage_collector` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/listeners/listener.py#L83'>airflow/listeners/listener.py:83:9:</a> WPS121 Local variable `_listener_manager` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/models/crypto.py#L83'>airflow/models/crypto.py:83:13:</a> WPS121 Local variable `_fernet` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/models/crypto.py#L85'>airflow/models/crypto.py:85:13:</a> WPS121 Local variable `_fernet` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/models/variable.py#L315'>airflow/models/variable.py:315:25:</a> WPS121 Local variable `_backend_name` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/serialization/serialized_objects.py#L1525'>airflow/serialization/serialized_objects.py:1525:13:</a> WPS121 Local variable `_operator_link_class_path` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/traces/otel_tracer.py#L117'>airflow/traces/otel_tracer.py:117:13:</a> WPS121 Local variable `_links` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/traces/otel_tracer.py#L150'>airflow/traces/otel_tracer.py:150:9:</a> WPS121 Local variable `_links` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/traces/otel_tracer.py#L202'>airflow/traces/otel_tracer.py:202:9:</a> WPS121 Local variable `_links` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/context.py#L249'>airflow/utils/context.py:249:9:</a> WPS121 Local variable `_asset_ref_names` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/operator_helpers.py#L121'>airflow/utils/operator_helpers.py:121:9:</a> WPS121 Local variable `_attr` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/task_instance_session.py#L41'>airflow/utils/task_instance_session.py:41:13:</a> WPS121 Local variable `__current_task_instance_session` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/task_instance_session.py#L49'>airflow/utils/task_instance_session.py:49:9:</a> WPS121 Local variable `__current_task_instance_session` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/task_instance_session.py#L64'>airflow/utils/task_instance_session.py:64:5:</a> WPS121 Local variable `__current_task_instance_session` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/task_instance_session.py#L68'>airflow/utils/task_instance_session.py:68:9:</a> WPS121 Local variable `__current_task_instance_session` is marked as unused but is used
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/www/auth.py#L83'>airflow/www/auth.py:83:13:</a> WPS121 Local variable `_permission_name` is marked as unused but is used
... 137 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/connectors/sqla/models.py#L461'>superset/connectors/sqla/models.py:461:17:</a> WPS121 Local variable `_columns` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/db_engine_specs/presto.py#L149'>superset/db_engine_specs/presto.py:149:13:</a> WPS121 Local variable `_column` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L71'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:71:29:</a> WPS121 Local variable `__from` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L72'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:72:29:</a> WPS121 Local variable `__to` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/models/helpers.py#L1650'>superset/models/helpers.py:1650:21:</a> WPS121 Local variable `_sql` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/models/helpers.py#L1651'>superset/models/helpers.py:1651:21:</a> WPS121 Local variable `_column_label` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/models/helpers.py#L1898'>superset/models/helpers.py:1898:25:</a> WPS121 Local variable `_since` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/models/helpers.py#L1898'>superset/models/helpers.py:1898:33:</a> WPS121 Local variable `_until` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/date_parser.py#L180'>superset/utils/date_parser.py:180:5:</a> WPS121 Local variable `_relative_start` is marked as unused but is used
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/date_parser.py#L181'>superset/utils/date_parser.py:181:5:</a> WPS121 Local variable `_relative_end` is marked as unused but is used
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L59'>examples/server/app/spectrogram/audio.py:59:13:</a> WPS121 Local variable `_f_carrier` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L60'>examples/server/app/spectrogram/audio.py:60:13:</a> WPS121 Local variable `_f_mod` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/audio.py#L61'>examples/server/app/spectrogram/audio.py:61:13:</a> WPS121 Local variable `_ind_mod` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:9:</a> WPS121 Local variable `_cwd` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L220'>src/bokeh/application/handlers/code_runner.py:220:9:</a> WPS121 Local variable `_sys_path` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L221'>src/bokeh/application/handlers/code_runner.py:221:9:</a> WPS121 Local variable `_sys_argv` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/static.py#L79'>src/bokeh/command/subcommands/static.py:79:9:</a> WPS121 Local variable `_allowed_keys` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L450'>src/bokeh/core/property/bases.py:450:9:</a> WPS121 Local variable `_type_params` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L484'>src/bokeh/io/notebook.py:484:5:</a> WPS121 Local variable `_NOTEBOOK_LOADED` is marked as unused but is used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/state.py#L232'>src/bokeh/io/state.py:232:9:</a> WPS121 Local variable `_STATE` is marked as unused but is used
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/analytics/tests/test_counts.py#L1035'>analytics/tests/test_counts.py:1035:9:</a> WPS121 Local variable `_1day` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/analytics/tests/test_counts.py#L1058'>analytics/tests/test_counts.py:1058:9:</a> WPS121 Local variable `_15day` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/analytics/tests/test_counts.py#L1108'>analytics/tests/test_counts.py:1108:9:</a> WPS121 Local variable `_15day` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/analytics/tests/test_counts.py#L985'>analytics/tests/test_counts.py:985:9:</a> WPS121 Local variable `_1day` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/corporate/lib/stripe.py#L1819'>corporate/lib/stripe.py:1819:17:</a> WPS121 Local variable `_price_per_license` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/zerver/data_import/slack.py#L496'>zerver/data_import/slack.py:496:5:</a> WPS121 Local variable `_default_timezone` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/zerver/lib/import_realm.py#L1081'>zerver/lib/import_realm.py:1081:13:</a> WPS121 Local variable `_cache` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/zerver/lib/markdown/__init__.py#L2674'>zerver/lib/markdown/__init__.py:2674:5:</a> WPS121 Local variable `_md_engine` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/zerver/lib/test_runner.py#L200'>zerver/lib/test_runner.py:200:9:</a> WPS121 Local variable `_worker_id` is marked as unused but is used
+ <a href='https://github.com/zulip/zulip/blob/e172c717f7b622175dc3a93f16ecec6d2a4c3bee/zerver/lib/transfer.py#L143'>zerver/lib/transfer.py:143:9:</a> WPS121 Local variable `_cache` is marked as unused but is used
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| WPS121 | 253 | 253 | 0 | 0 | 0 |

</p>
</details>




---
