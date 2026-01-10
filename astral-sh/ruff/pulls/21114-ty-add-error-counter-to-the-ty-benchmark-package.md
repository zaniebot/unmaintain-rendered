```yaml
number: 21114
title: "[ty] Add error counter to the ty benchmark package"
type: pull_request
state: closed
author: alex-gregory-ds
labels:
  - testing
  - ty
assignees: []
draft: true
base: main
head: ty-benchmark-error-counter
created_at: 2025-10-28T21:04:49Z
updated_at: 2025-11-23T12:17:11Z
url: https://github.com/astral-sh/ruff/pull/21114
synced_at: 2026-01-10T16:48:01Z
```

# [ty] Add error counter to the ty benchmark package

---

_Pull request opened by @alex-gregory-ds on 2025-10-28 21:04_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Related to https://github.com/astral-sh/ty/issues/241.

This adds a counter to count the number of errors that are produced by ty on each of the benchmark repositories in `ty_benchmark`. For simplicity, we count the numer of lines produce by ty (when using `--output-format=concise`) as a proxy for the number of errors instead of counting the actual number of errors.

## Test Plan

<!-- How was it tested? -->

This is a draft so it's still a work in progress.


---

_Label `testing` added by @AlexWaygood on 2025-10-29 19:56_

---

_Label `ty` added by @AlexWaygood on 2025-10-29 19:56_

---

_Comment by @github-actions[bot] on 2025-10-29 20:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -227 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -227 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L547'>airflow-core/src/airflow/cli/commands/dag_command.py:547:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L578'>airflow-core/src/airflow/cli/commands/dag_command.py:578:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L616'>airflow-core/src/airflow/cli/commands/dag_command.py:616:25:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L155'>airflow-core/src/airflow/dag_processing/dagbag.py:155:36:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L245'>airflow-core/src/airflow/dag_processing/dagbag.py:245:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L561'>airflow-core/src/airflow/dag_processing/dagbag.py:561:28:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L87'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:87:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L152'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:152:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L145'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:145:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/unit/dag_processing/test_processor.py#L541'>airflow-core/tests/unit/dag_processing/test_processor.py:541:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/unit/utils/test_db_cleanup.py#L684'>airflow-core/tests/unit/utils/test_db_cleanup.py:684:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/devel-common/src/tests_common/pytest_plugin.py#L1251'>devel-common/src/tests_common/pytest_plugin.py:1251:24:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/devel-common/src/tests_common/pytest_plugin.py#L2805'>devel-common/src/tests_common/pytest_plugin.py:2805:16:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/performance/src/performance_dags/performance_dag/performance_dag.py#L235'>performance/src/performance_dags/performance_dag/performance_dag.py:235:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py#L33'>providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py:33:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/transfers/test_base.py#L42'>providers/amazon/tests/unit/amazon/aws/transfers/test_base.py:42:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py#L268'>providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py:268:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/druid/tests/system/apache/druid/example_druid.py#L31'>providers/apache/druid/tests/system/apache/druid/example_druid.py:31:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L201'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:201:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L887'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:887:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 199 additional changes omitted for rule AIR311
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L123'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:123:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L966'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:966:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1715'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1715:18:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 224 | 0 | 224 | 0 | 0 |
| AIR301 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -227 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -227 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L547'>airflow-core/src/airflow/cli/commands/dag_command.py:547:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L578'>airflow-core/src/airflow/cli/commands/dag_command.py:578:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/cli/commands/dag_command.py#L616'>airflow-core/src/airflow/cli/commands/dag_command.py:616:25:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L155'>airflow-core/src/airflow/dag_processing/dagbag.py:155:36:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L245'>airflow-core/src/airflow/dag_processing/dagbag.py:245:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/src/airflow/dag_processing/dagbag.py#L561'>airflow-core/src/airflow/dag_processing/dagbag.py:561:28:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L87'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:87:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L152'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:152:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L145'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:145:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/unit/dag_processing/test_processor.py#L541'>airflow-core/tests/unit/dag_processing/test_processor.py:541:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/airflow-core/tests/unit/utils/test_db_cleanup.py#L684'>airflow-core/tests/unit/utils/test_db_cleanup.py:684:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/devel-common/src/tests_common/pytest_plugin.py#L1251'>devel-common/src/tests_common/pytest_plugin.py:1251:24:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/devel-common/src/tests_common/pytest_plugin.py#L2805'>devel-common/src/tests_common/pytest_plugin.py:2805:16:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/performance/src/performance_dags/performance_dag/performance_dag.py#L235'>performance/src/performance_dags/performance_dag/performance_dag.py:235:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py#L33'>providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py:33:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/transfers/test_base.py#L42'>providers/amazon/tests/unit/amazon/aws/transfers/test_base.py:42:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py#L268'>providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py:268:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/druid/tests/system/apache/druid/example_druid.py#L31'>providers/apache/druid/tests/system/apache/druid/example_druid.py:31:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L201'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:201:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L887'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:887:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 199 additional changes omitted for rule AIR311
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L123'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:123:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L966'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:966:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/599659aa05ee766c8421c53516861c19649c73dc/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1715'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1715:18:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 224 | 0 | 224 | 0 | 0 |
| AIR301 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[ty] Add error counter to the ty bencmark package" to "[ty] Add error counter to the ty benchmark package" by @AlexWaygood on 2025-10-29 20:08_

---

_Comment by @alex-gregory-ds on 2025-10-31 20:22_

Can anyone give me some guidance on where to go from here please? I have a function that counts the number of errors (number of lines) produced by ty. I assume I need to log the number of errors somewhere but:

* I cannot find where `ty_benchmark` is run in the CI/CD pipeline.
* I cannot see how the current Hyperfine benchmark results are logged.

---

_Comment by @MichaReiser on 2025-11-07 23:44_

The `ty_benchmark` doesn't run on CI. It's a script that we run manually when comparing different type checkers. 

>  I assume I need to log the number of errors somewhere but:

I think what we want is to hard code a rough number of expected diagnostics and compare them with the diagnostics count when running ty. Logging the diagnostic count when there's a mismatch would certainly be useful.

However, I'm not entirely convinced that this is indeed necessary. We could also run ty with `--exit-zero` and have `hyperfine` assert that the exit code is 0.

---

_Comment by @alex-gregory-ds on 2025-11-08 12:03_

Thanks for the suggestions. This PR includes an `error_count.json` file and every time the benchmark is run, a new file is created called `error_count_new.json`. At the end of the benchmark run, the difference between the two JSON files is printed to the terminal. Here is an example:

```
+--------------------------------------------------------------------------------------+
|                                 Error Count Summary                                  |
+-----------------+----------------------+----------------------+----------------------+
|                 | Old (6a92f1d9)       | New (3ffe3eea)       | Difference           |
|                 | 2025-11-08 11:55:29  | 2025-11-08 12:09:53  |                      |
+-----------------+----------------------+----------------------+----------------------+
| black (cold)    | 44                   | 44                   | 0                    |
| isort (cold)    | 21                   | 21                   | 0                    |
| jinja (cold)    | 81                   | 81                   | 0                    |
+-----------------+----------------------+----------------------+----------------------+
```

Let me know if this is right direction or if it's too much for what you need. If this isn't the right direction, I am happy to close this and try again :).

Currently, I am not running `ty` with `--exit-zero`, and I am not doing this with `hyperfine` but I am happy to make those changes if you'd prefer.

---

_Comment by @MichaReiser on 2025-11-17 07:29_

CC: @AlexWaygood 

---

_Comment by @MichaReiser on 2025-11-20 13:09_

Thank you @alex-gregory-ds for working on this. 

I realized while working on adding and updating our benchmarks that a much easier approach is just to snapshot the output, and I implemented this as part of https://github.com/astral-sh/ruff/pull/21536. It's certainly a bit verbose, but it has the added benefit that it allows comparison between versions. 

I'm sorry that I misled you towards comparing the error counts instead. I might use your approach if the team decides that my solution is too verbose. But for now, I'll close this PR



---

_Closed by @MichaReiser on 2025-11-20 13:09_

---

_Comment by @alex-gregory-ds on 2025-11-23 12:17_

> I'm sorry that I misled you towards comparing the error counts instead. I might use your approach if the team decides that my solution is too verbose. But for now, I'll close this PR.

No problem at all. Thank you for getting back to me.

---
