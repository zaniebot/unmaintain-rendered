```yaml
number: 21093
title: "[`airflow`] warning `airflow....DAG.create_dagrun` has been removed (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
assignees: []
merged: true
base: main
head: extend-AIR301-create-dagrun
created_at: 2025-10-27T07:51:35Z
updated_at: 2025-10-29T14:57:37Z
url: https://github.com/astral-sh/ruff/pull/21093
synced_at: 2026-01-12T15:57:16Z
```

# [`airflow`] warning `airflow....DAG.create_dagrun` has been removed (`AIR301`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
* Warn that `airflow....DAG.create_dagrun` has been removed

## Test Plan

<!-- How was it tested? -->
update accordingly and reorganize test cases

---

_Comment by @github-actions[bot] on 2025-10-27 08:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+227 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+227 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L547'>airflow-core/src/airflow/cli/commands/dag_command.py:547:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L578'>airflow-core/src/airflow/cli/commands/dag_command.py:578:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L616'>airflow-core/src/airflow/cli/commands/dag_command.py:616:25:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L155'>airflow-core/src/airflow/dag_processing/dagbag.py:155:36:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L245'>airflow-core/src/airflow/dag_processing/dagbag.py:245:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L561'>airflow-core/src/airflow/dag_processing/dagbag.py:561:28:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L87'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:87:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L152'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:152:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L145'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:145:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/unit/dag_processing/test_processor.py#L541'>airflow-core/tests/unit/dag_processing/test_processor.py:541:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/unit/utils/test_db_cleanup.py#L684'>airflow-core/tests/unit/utils/test_db_cleanup.py:684:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/devel-common/src/tests_common/pytest_plugin.py#L1251'>devel-common/src/tests_common/pytest_plugin.py:1251:24:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/devel-common/src/tests_common/pytest_plugin.py#L2805'>devel-common/src/tests_common/pytest_plugin.py:2805:16:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/performance/src/performance_dags/performance_dag/performance_dag.py#L235'>performance/src/performance_dags/performance_dag/performance_dag.py:235:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py#L33'>providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py:33:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/transfers/test_base.py#L42'>providers/amazon/tests/unit/amazon/aws/transfers/test_base.py:42:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py#L268'>providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py:268:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/druid/tests/system/apache/druid/example_druid.py#L31'>providers/apache/druid/tests/system/apache/druid/example_druid.py:31:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L201'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:201:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L887'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:887:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 199 additional changes omitted for rule AIR311
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L123'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:123:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L966'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:966:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1715'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1715:18:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 224 | 224 | 0 | 0 | 0 |
| AIR301 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+227 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+227 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L547'>airflow-core/src/airflow/cli/commands/dag_command.py:547:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L578'>airflow-core/src/airflow/cli/commands/dag_command.py:578:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/cli/commands/dag_command.py#L616'>airflow-core/src/airflow/cli/commands/dag_command.py:616:25:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L155'>airflow-core/src/airflow/dag_processing/dagbag.py:155:36:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L245'>airflow-core/src/airflow/dag_processing/dagbag.py:245:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/src/airflow/dag_processing/dagbag.py#L561'>airflow-core/src/airflow/dag_processing/dagbag.py:561:28:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L87'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:87:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L152'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:152:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L145'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:145:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/unit/dag_processing/test_processor.py#L541'>airflow-core/tests/unit/dag_processing/test_processor.py:541:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/airflow-core/tests/unit/utils/test_db_cleanup.py#L684'>airflow-core/tests/unit/utils/test_db_cleanup.py:684:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/devel-common/src/tests_common/pytest_plugin.py#L1251'>devel-common/src/tests_common/pytest_plugin.py:1251:24:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/devel-common/src/tests_common/pytest_plugin.py#L2805'>devel-common/src/tests_common/pytest_plugin.py:2805:16:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/performance/src/performance_dags/performance_dag/performance_dag.py#L235'>performance/src/performance_dags/performance_dag/performance_dag.py:235:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py#L33'>providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py:33:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/transfers/test_base.py#L42'>providers/amazon/tests/unit/amazon/aws/transfers/test_base.py:42:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py#L268'>providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py:268:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/druid/tests/system/apache/druid/example_druid.py#L31'>providers/apache/druid/tests/system/apache/druid/example_druid.py:31:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L201'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:201:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L887'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:887:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 199 additional changes omitted for rule AIR311
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L123'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:123:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L966'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:966:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/a6d3e2d00639f95e619647329668f62a68b7f1f2/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1715'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1715:18:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 224 | 224 | 0 | 0 | 0 |
| AIR301 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "feat(AIR301): warn that . has been removed" to "[`airflow`] warning `airflow....DAG.create_dagrun` has been removed" by @Lee-W on 2025-10-27 08:08_

---

_Renamed from "[`airflow`] warning `airflow....DAG.create_dagrun` has been removed" to "[`airflow`] warning `airflow....DAG.create_dagrun` has been removed (`AIR301`)" by @Lee-W on 2025-10-27 08:08_

---

_Comment by @MichaReiser on 2025-10-27 09:08_

Now that the rule is stable, changes like this may need to be gated behind preview and go through our normal stabilization period. 

I think the change here is fine as it isn't a significant expansion of the rule's scope:



---

_Label `rule` added by @MichaReiser on 2025-10-27 09:08_

---

_@MichaReiser reviewed on 2025-10-27 09:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:495 on 2025-10-27 09:09_

Would it be better to be explicit from which module `DAG` was removed to avoid false positives instead of allowing any `DAG` symbol imported from anywhere within airflow (e.g. this also flags `airflow.providers.my_provider.DAG`)

---

_Comment by @Lee-W on 2025-10-27 12:40_

> Now that the rule is stable, changes like this may need to be gated behind preview and go through our normal stabilization period.
> 
> I think the change here is fine as it isn't a significant expansion of the rule's scope:

We had a discussion with @ntBre previously. Our conclusion back then was this kind of small ones are fine.

But if we want to make it gated behind preview, what would be the process? Do we need to extract these logic to somewhere so that they can be activated through preview flag? Thanks!

---

_Comment by @ntBre on 2025-10-27 12:44_

> But if we want to make it gated behind preview, what would be the process? Do we need to extract these logic to somewhere so that they can be activated through preview flag? Thanks!

It should be pretty easy to add guards to the `match` arms and a new preview function like in [`preview.rs`](https://github.com/astral-sh/ruff/blob/c0fb235a703684bca1cc1767015bb7e19ab1c22f/crates/ruff_linter/src/preview.rs#L16-L19).

But I agree that this change seems fine without preview.

---

_Comment by @Lee-W on 2025-10-27 12:48_

Cool! This is super helpful! We might have some minor ones like this. Please let me know if any of them need to be in preview mode. Thank you!

---

_@Lee-W reviewed on 2025-10-27 12:49_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:495 on 2025-10-27 12:49_

it's unlikely, but definiely better. Let me make the change.

---

_@Lee-W reviewed on 2025-10-28 00:01_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:495 on 2025-10-28 00:01_

just updated. Thanks!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:495 on 2025-10-28 18:27_

Can you double-check if you pushed your most recent changes? 

---

_@MichaReiser reviewed on 2025-10-28 18:27_

---

_@Lee-W reviewed on 2025-10-29 01:09_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:495 on 2025-10-29 01:09_

Hi, sorry for the mess, it might be missed due to wrong rebasing. Just updated it and checked again. Thanks for reminder!

---

_@MichaReiser approved on 2025-10-29 14:57_

---

_Merged by @MichaReiser on 2025-10-29 14:57_

---

_Closed by @MichaReiser on 2025-10-29 14:57_

---
