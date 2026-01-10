```yaml
number: 10621
title: "`DTZ` rules: Clarify error messages and docs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - rule
assignees: []
merged: true
base: main
head: dtz-clarify
created_at: 2024-03-26T18:53:42Z
updated_at: 2024-07-01T10:29:51Z
url: https://github.com/astral-sh/ruff/pull/10621
synced_at: 2026-01-10T21:55:59Z
```

# `DTZ` rules: Clarify error messages and docs

---

_Pull request opened by @AlexWaygood on 2024-03-26 18:53_

Fixes #10251. Closes #10590.

## Summary

This PR builds upon #10590, by @cclauss. The problem with DTZ005 pointed out in #10251 is really a common problem to most of the DTZ rules, so it makes sense to tackle them all at the same time.

Changes made:

- Clearly state in the documentation that passing `tz=None` is just as bad as not passing a `tz=` argument, from the perspective of these rules.
- Clearly state in the error messages exactly what the user is doing wrong, if the user is passing `tz=None` rather than failing to pass a `tz=` argument at all.
- Make error messages more concise, and separate out the suggested remedy from the thing that the user is identified as doing wrong.

## Test Plan

`cargo test`

---

Co-authored-by: Christian Clauss <cclauss@me.com>

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-26 18:53_

---

_Comment by @github-actions[bot] on 2024-03-26 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+868 -868 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+797 -797 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L415'>airflow/dag_processing/manager.py:415:62:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L415'>airflow/dag_processing/manager.py:415:62:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L419'>airflow/dag_processing/manager.py:419:68:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L419'>airflow/dag_processing/manager.py:419:68:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping.py#L27'>airflow/example_dags/example_dynamic_task_mapping.py:27:60:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping.py#L27'>airflow/example_dags/example_dynamic_task_mapping.py:27:60:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L56'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:56:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L56'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:56:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_latest_only.py#L31'>airflow/example_dags/example_latest_only.py:31:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_latest_only.py#L31'>airflow/example_dags/example_latest_only.py:31:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_local_kubernetes_executor.py#L48'>airflow/example_dags/example_local_kubernetes_executor.py:48:20:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
... 1280 additional changes omitted for rule DTZ001
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L145'>airflow/example_dags/example_params_ui_tutorial.py:145:16:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L145'>airflow/example_dags/example_params_ui_tutorial.py:145:16:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L152'>airflow/example_dags/example_params_ui_tutorial.py:152:16:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L152'>airflow/example_dags/example_params_ui_tutorial.py:152:16:</a> DTZ011 `datetime.date.today()` used
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L59'>airflow/macros/__init__.py:59:10:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L59'>airflow/macros/__init__.py:59:10:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L76'>airflow/macros/__init__.py:76:12:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L76'>airflow/macros/__init__.py:76:12:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/models/dagbag.py#L298'>airflow/models/dagbag.py:298:41:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/models/dagbag.py#L298'>airflow/models/dagbag.py:298:41:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
... 194 additional changes omitted for rule DTZ005
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L113'>airflow/providers/apache/hive/macros/hive.py:113:18:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L113'>airflow/providers/apache/hive/macros/hive.py:113:18:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L114'>airflow/providers/apache/hive/macros/hive.py:114:21:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
... 34 additional changes omitted for rule DTZ007
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/kylin/operators/kylin_cube.py#L147'>airflow/providers/apache/kylin/operators/kylin_cube.py:147:22:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
... 22 additional changes omitted for rule DTZ006
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:61:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:61:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:94:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
... 6 additional changes omitted for rule DTZ011
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/sensors/time_sensor.py#L70'>airflow/sensors/time_sensor.py:70:39:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/sensors/time_sensor.py#L70'>airflow/sensors/time_sensor.py:70:39:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L52'>airflow/utils/log/file_processor_handler.py:52:26:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L52'>airflow/utils/log/file_processor_handler.py:52:26:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L68'>airflow/utils/log/file_processor_handler.py:68:29:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L68'>airflow/utils/log/file_processor_handler.py:68:29:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L70'>airflow/utils/log/file_processor_handler.py:70:30:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
... 20 additional changes omitted for rule DTZ002
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/tests/providers/sftp/triggers/test_sftp.py#L152'>tests/providers/sftp/triggers/test_sftp.py:152:38:</a> DTZ012 The use of `datetime.date.fromtimestamp()` is not allowed, use `datetime.datetime.fromtimestamp(ts, tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/tests/providers/sftp/triggers/test_sftp.py#L152'>tests/providers/sftp/triggers/test_sftp.py:152:38:</a> DTZ012 `datetime.date.fromtimestamp()` used
... 1550 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+70 -70 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L21'>docs/bokeh/source/conf.py:21:8:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L21'>docs/bokeh/source/conf.py:21:8:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L17'>examples/basic/annotations/polygon_annotation.py:17:14:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L17'>examples/basic/annotations/polygon_annotation.py:17:14:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L20'>examples/basic/annotations/polygon_annotation.py:20:12:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L20'>examples/basic/annotations/polygon_annotation.py:20:12:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/span.py#L18'>examples/basic/annotations/span.py:18:27:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/span.py#L18'>examples/basic/annotations/span.py:18:27:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
... 105 additional changes omitted for rule DTZ001
... 130 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/8b638d4d6c9935a654e5820b40b50adf166bb18a/packaging/docker/entrypoint.py#L26'>packaging/docker/entrypoint.py:26:20:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/rotki/rotki/blob/8b638d4d6c9935a654e5820b40b50adf166bb18a/packaging/docker/entrypoint.py#L26'>packaging/docker/entrypoint.py:26:20:</a> DTZ002 `datetime.datetime.today()` used
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ001 | 1396 | 698 | 698 | 0 | 0 |
| DTZ005 | 218 | 109 | 109 | 0 | 0 |
| DTZ007 | 40 | 20 | 20 | 0 | 0 |
| DTZ006 | 34 | 17 | 17 | 0 | 0 |
| DTZ002 | 30 | 15 | 15 | 0 | 0 |
| DTZ011 | 16 | 8 | 8 | 0 | 0 |
| DTZ012 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+868 -868 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+797 -797 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L415'>airflow/dag_processing/manager.py:415:62:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L415'>airflow/dag_processing/manager.py:415:62:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L419'>airflow/dag_processing/manager.py:419:68:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/dag_processing/manager.py#L419'>airflow/dag_processing/manager.py:419:68:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping.py#L27'>airflow/example_dags/example_dynamic_task_mapping.py:27:60:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping.py#L27'>airflow/example_dags/example_dynamic_task_mapping.py:27:60:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L56'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:56:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L56'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:56:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_latest_only.py#L31'>airflow/example_dags/example_latest_only.py:31:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_latest_only.py#L31'>airflow/example_dags/example_latest_only.py:31:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_local_kubernetes_executor.py#L48'>airflow/example_dags/example_local_kubernetes_executor.py:48:20:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
... 1280 additional changes omitted for rule DTZ001
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L145'>airflow/example_dags/example_params_ui_tutorial.py:145:16:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L145'>airflow/example_dags/example_params_ui_tutorial.py:145:16:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L152'>airflow/example_dags/example_params_ui_tutorial.py:152:16:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/example_dags/example_params_ui_tutorial.py#L152'>airflow/example_dags/example_params_ui_tutorial.py:152:16:</a> DTZ011 `datetime.date.today()` used
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L59'>airflow/macros/__init__.py:59:10:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L59'>airflow/macros/__init__.py:59:10:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L76'>airflow/macros/__init__.py:76:12:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/macros/__init__.py#L76'>airflow/macros/__init__.py:76:12:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/models/dagbag.py#L298'>airflow/models/dagbag.py:298:41:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/models/dagbag.py#L298'>airflow/models/dagbag.py:298:41:</a> DTZ006 `datetime.datetime.fromtimestamp()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
... 194 additional changes omitted for rule DTZ005
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L113'>airflow/providers/apache/hive/macros/hive.py:113:18:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L113'>airflow/providers/apache/hive/macros/hive.py:113:18:</a> DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/hive/macros/hive.py#L114'>airflow/providers/apache/hive/macros/hive.py:114:21:</a> DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
... 34 additional changes omitted for rule DTZ007
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/apache/kylin/operators/kylin_cube.py#L147'>airflow/providers/apache/kylin/operators/kylin_cube.py:147:22:</a> DTZ006 The use of `datetime.datetime.fromtimestamp()` without `tz` argument is not allowed
... 22 additional changes omitted for rule DTZ006
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:61:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:61:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py#L108'>airflow/providers/google/cloud/operators/cloud_storage_transfer_service.py:108:94:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
... 6 additional changes omitted for rule DTZ011
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/sensors/time_sensor.py#L70'>airflow/sensors/time_sensor.py:70:39:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/sensors/time_sensor.py#L70'>airflow/sensors/time_sensor.py:70:39:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L52'>airflow/utils/log/file_processor_handler.py:52:26:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L52'>airflow/utils/log/file_processor_handler.py:52:26:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L68'>airflow/utils/log/file_processor_handler.py:68:29:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L68'>airflow/utils/log/file_processor_handler.py:68:29:</a> DTZ002 `datetime.datetime.today()` used
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/airflow/utils/log/file_processor_handler.py#L70'>airflow/utils/log/file_processor_handler.py:70:30:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
... 20 additional changes omitted for rule DTZ002
- <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/tests/providers/sftp/triggers/test_sftp.py#L152'>tests/providers/sftp/triggers/test_sftp.py:152:38:</a> DTZ012 The use of `datetime.date.fromtimestamp()` is not allowed, use `datetime.datetime.fromtimestamp(ts, tz=).date()` instead
+ <a href='https://github.com/apache/airflow/blob/6ebbda515eee4b9739997290132eb62d775907fe/tests/providers/sftp/triggers/test_sftp.py#L152'>tests/providers/sftp/triggers/test_sftp.py:152:38:</a> DTZ012 `datetime.date.fromtimestamp()` used
... 1550 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+70 -70 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L21'>docs/bokeh/source/conf.py:21:8:</a> DTZ011 The use of `datetime.date.today()` is not allowed, use `datetime.datetime.now(tz=).date()` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L21'>docs/bokeh/source/conf.py:21:8:</a> DTZ011 `datetime.date.today()` used
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 `datetime.datetime.now()` called without a `tz` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L17'>examples/basic/annotations/polygon_annotation.py:17:14:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L17'>examples/basic/annotations/polygon_annotation.py:17:14:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L20'>examples/basic/annotations/polygon_annotation.py:20:12:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/polygon_annotation.py#L20'>examples/basic/annotations/polygon_annotation.py:20:12:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/span.py#L18'>examples/basic/annotations/span.py:18:27:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/span.py#L18'>examples/basic/annotations/span.py:18:27:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
... 105 additional changes omitted for rule DTZ001
... 130 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/8b638d4d6c9935a654e5820b40b50adf166bb18a/packaging/docker/entrypoint.py#L26'>packaging/docker/entrypoint.py:26:20:</a> DTZ002 The use of `datetime.datetime.today()` is not allowed, use `datetime.datetime.now(tz=)` instead
+ <a href='https://github.com/rotki/rotki/blob/8b638d4d6c9935a654e5820b40b50adf166bb18a/packaging/docker/entrypoint.py#L26'>packaging/docker/entrypoint.py:26:20:</a> DTZ002 `datetime.datetime.today()` used
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ001 | 1396 | 698 | 698 | 0 | 0 |
| DTZ005 | 218 | 109 | 109 | 0 | 0 |
| DTZ007 | 40 | 20 | 20 | 0 | 0 |
| DTZ006 | 34 | 17 | 17 | 0 | 0 |
| DTZ002 | 30 | 15 | 15 | 0 | 0 |
| DTZ011 | 16 | 8 | 8 | 0 | 0 |
| DTZ012 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Label `documentation` added by @AlexWaygood on 2024-03-26 19:09_

---

_Label `rule` added by @AlexWaygood on 2024-03-26 19:09_

---

_@cclauss reviewed on 2024-03-26 19:14_

Nice!

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_fromtimestamp.rs`:59 on 2024-03-26 20:24_

is it not allowed (raise an error) or it is just a bad practice? 

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_fromtimestamp.rs`:62 on 2024-03-26 20:26_

```suggestion
                "Passing `tz=None` is not recommended, as it creates a naive datetime object"
```
or passing `tz=None` raise an error?

---

_@VascoSch92 reviewed on 2024-03-26 20:27_

---

_@AlexWaygood reviewed on 2024-03-26 21:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_fromtimestamp.rs`:59 on 2024-03-26 21:11_

Great question. The error messages we were giving were much more verbose than is normal for our messages, and were still pretty unclear. I tidied them up in https://github.com/astral-sh/ruff/pull/10621/commits/7beb878e274ee091ed2bd7c138906565253c7951.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ005_DTZ005.py.snap`:4 on 2024-03-27 08:12_

Nit: 
I think we should either remove `argument` and change it to `tz=<timezone>` or keep the sentence as is but change `tz=` to `tz` because the argument name is `tz` (and not `tz=`). I also thought at first that `tz=` is a typo

```suggestion
DTZ005.py:4:1: DTZ005 `datetime.datetime.now()` called without a `tz` argument
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ007_DTZ007.py.snap`:22 on 2024-03-27 08:14_

Nit: I don't know if there's precedence for "incomplete" syntax in other help texts. I recommend changing it to one of

```
.replace(tzinfo=...) 
.replace(tzinfo=<timezone>)
```

to make it clear, that a value must follow after the `=`
 

---

_@MichaReiser approved on 2024-03-27 08:14_

---

_Merged by @AlexWaygood on 2024-03-27 19:42_

---

_Closed by @AlexWaygood on 2024-03-27 19:42_

---

_Branch deleted on 2024-07-01 10:29_

---
