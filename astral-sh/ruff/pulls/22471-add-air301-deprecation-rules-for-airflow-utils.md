```yaml
number: 22471
title: Add AIR301 deprecation rules for airflow.utils module classes
type: pull_request
state: open
author: jashparekh
labels:
  - rule
assignees: []
base: main
head: main
created_at: 2026-01-09T03:50:06Z
updated_at: 2026-01-13T02:33:49Z
url: https://github.com/astral-sh/ruff/pull/22471
synced_at: 2026-01-13T03:19:47Z
```

# Add AIR301 deprecation rules for airflow.utils module classes

---

_@jashparekh_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add detection and auto-fix for deprecated classes in airflow.utils:
- setup_teardown: BaseSetupTeardownContext, SetupTeardownContext
- xcom: XCOM_RETURN_KEY
- task_group: TaskGroup
- timeout: timeout
- weight_rule: WeightRule, DB_SAFE_MINIMUM, DB_SAFE_MAXIMUM, db_safe_priority
- decorators: remove_task_decorator, fixup_decorator_warning_stack

Fixes: https://github.com/astral-sh/ruff/issues/22459

## Test Plan

Added test cases in AIR301_names_fix.py for all new deprecations
Updated snapshots with cargo insta accept
All tests pass: cargo nextest run -p ruff_linter -- airflow
Clippy passes: cargo clippy --workspace --all-targets --all-features -- -D warnings
Pre-commit passes: uvx pre-commit run -a


---

_Renamed from "[ty] Add AIR301 deprecation rules for airflow.utils module classes" to "Add AIR301 deprecation rules for airflow.utils module classes" by @MichaReiser on 2026-01-09 11:03_

---

_Comment by @MichaReiser on 2026-01-09 11:04_

CC: @Lee-W

---

_Label `rule` added by @MichaReiser on 2026-01-09 11:05_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 19:48_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1924'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1924:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1945'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1945:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1958'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1958:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py#L683'>providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py:683:14:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py#L684'>providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py:684:15:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/decorators/test_python.py#L477'>providers/standard/tests/unit/standard/decorators/test_python.py:477:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/operators/test_branch_operator.py#L311'>providers/standard/tests/unit/standard/operators/test_branch_operator.py:311:24:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py#L130'>providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py:130:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py#L150'>providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py:150:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L171'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:171:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L200'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:200:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L387'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:387:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1924'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1924:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1945'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1945:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/google/tests/unit/google/cloud/operators/test_bigquery.py#L1958'>providers/google/tests/unit/google/cloud/operators/test_bigquery.py:1958:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py#L683'>providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py:683:14:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py#L684'>providers/openlineage/tests/unit/openlineage/plugins/test_adapter.py:684:15:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/decorators/test_python.py#L477'>providers/standard/tests/unit/standard/decorators/test_python.py:477:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/operators/test_branch_operator.py#L311'>providers/standard/tests/unit/standard/operators/test_branch_operator.py:311:24:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py#L130'>providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py:130:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py#L150'>providers/standard/tests/unit/standard/sensors/test_external_task_sensor.py:150:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L171'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:171:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L200'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:200:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4b8d9f35611c2c81399a9ac5b12daca49602e5f5/providers/standard/tests/unit/standard/utils/test_sensor_helper.py#L387'>providers/standard/tests/unit/standard/utils/test_sensor_helper.py:387:18:</a> AIR301 `airflow.utils.task_group.TaskGroup` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @Lee-W on 2026-01-13 02:33_

The list is not yet confirmed, and we're working on making it an AIR321 instead since some of them are 3.1.0+ instead of 3.0.0+

---
