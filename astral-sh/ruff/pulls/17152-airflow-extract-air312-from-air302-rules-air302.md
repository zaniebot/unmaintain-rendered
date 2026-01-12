```yaml
number: 17152
title: "[`airflow`] Extract `AIR312` from `AIR302` rules (`AIR302`, `AIR312`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extract-suggested-AIR-changes
created_at: 2025-04-02T14:34:18Z
updated_at: 2025-04-09T14:43:07Z
url: https://github.com/astral-sh/ruff/pull/17152
synced_at: 2026-01-12T15:56:00Z
```

# [`airflow`] Extract `AIR312` from `AIR302` rules (`AIR302`, `AIR312`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

As discussed in https://github.com/astral-sh/ruff/issues/14626#issuecomment-2766146129, we're to separate suggested changes from required changes.

The following symbols has been moved to AIR312 from AIR302. They still work in Airflow 3.0, but they're suggested to be changed as they're expected to be removed in future version

```python
from airflow.hooks.filesystem import FSHook
from airflow.hooks.package_index import PackageIndexHook
from airflow.hooks.subprocess import (SubprocessHook, SubprocessResult, working_directory)
from airflow.operators.bash import BashOperator
from airflow.operators.datetime import BranchDateTimeOperator, target_times_as_dates
from airflow.operators.trigger_dagrun import TriggerDagRunLink, TriggerDagRunOperator
from airflow.operators.empty import EmptyOperator
from airflow.operators.latest_only import LatestOnlyOperator
from airflow.operators.python import (BranchPythonOperator, PythonOperator, PythonVirtualenvOperator, ShortCircuitOperator)
from airflow.operators.weekday import BranchDayOfWeekOperator
from airflow.sensors.date_time import DateTimeSensor, DateTimeSensorAsync
from airflow.sensors.external_task import ExternalTaskMarker, ExternalTaskSensor, ExternalTaskSensorLink
from airflow.sensors.filesystem import FileSensor
from airflow.sensors.time_sensor import TimeSensor, TimeSensorAsync
from airflow.sensors.time_delta import TimeDeltaSensor, TimeDeltaSensorAsync, WaitSensor
from airflow.sensors.weekday import DayOfWeekSensor
from airflow.triggers.external_task import DagStateTrigger, WorkflowTrigger
from airflow.triggers.file import FileTrigger
from airflow.triggers.temporal import DateTimeTrigger, TimeDeltaTrigger
```

## Test Plan

<!-- How was it tested? -->

The test fixture has been updated acccordingly


---

_Assigned to @ntBre by @ntBre on 2025-04-03 22:14_

---

_Renamed from "Extract suggested air changes" to "[airflow] Extract AIR312 from AIR302 rules (AIR302, AIR312)" by @Lee-W on 2025-04-08 14:23_

---

_Marked ready for review by @Lee-W on 2025-04-08 14:23_

---

_Comment by @github-actions[bot] on 2025-04-08 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+13 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/airflow-core/tests/unit/utils/test_dot_renderer.py#L103'>airflow-core/tests/unit/utils/test_dot_renderer.py:103:22:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/airflow-core/tests/unit/utils/test_dot_renderer.py#L103'>airflow-core/tests/unit/utils/test_dot_renderer.py:103:22:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/airflow-core/tests/unit/utils/test_dot_renderer.py#L83'>airflow-core/tests/unit/utils/test_dot_renderer.py:83:22:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/airflow-core/tests/unit/utils/test_dot_renderer.py#L83'>airflow-core/tests/unit/utils/test_dot_renderer.py:83:22:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/performance/src/performance_dags/performance_dag/performance_dag.py#L111'>performance/src/performance_dags/performance_dag/performance_dag.py:111:13:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/performance/src/performance_dags/performance_dag/performance_dag.py#L111'>performance/src/performance_dags/performance_dag/performance_dag.py:111:13:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR312 `airflow.operators.bash.BashOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR312 `airflow.operators.bash.BashOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L162'>providers/tests/standard/utils/test_sensor_helper.py:162:17:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L166'>providers/tests/standard/utils/test_sensor_helper.py:166:21:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L194'>providers/tests/standard/utils/test_sensor_helper.py:194:21:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L224'>providers/tests/standard/utils/test_sensor_helper.py:224:13:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L257'>providers/tests/standard/utils/test_sensor_helper.py:257:13:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L313'>providers/tests/standard/utils/test_sensor_helper.py:313:17:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/tests/standard/utils/test_sensor_helper.py#L377'>providers/tests/standard/utils/test_sensor_helper.py:377:21:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/ydb/tests/system/ydb/example_ydb.py#L111'>providers/ydb/tests/system/ydb/example_ydb.py:111:23:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/ad2e80d2997dc46ca175a39a0a8b8c216361323f/providers/ydb/tests/system/ydb/example_ydb.py#L111'>providers/ydb/tests/system/ydb/example_ydb.py:111:23:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR312 | 13 | 13 | 0 | 0 | 0 |
| AIR302 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:754 on 2025-04-08 18:39_

I don't think `rustfmt` can handle the long `match` expressions very well, but I *think* it would usually do something more like this:

```rust
["airflow", "operators", "dagrun_operator", rest @ (
    "TriggerDagRunLink"
    |"TriggerDagRunOperator"
)
```

with spaces around the `@` and the `|` on the next line.

Again, I'm not too worried about manually fixing all of these since `rustfmt` is missing them, but if you happen to notice them, it could be nice to fix.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:13 on 2025-04-08 18:41_

```suggestion
/// Checks for uses of Airflow functions and values that have been moved to its providers
/// but still have a compatibility layer (e.g., `apache-airflow-providers-standard`).
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:19 on 2025-04-08 18:43_

```suggestion
/// they are expected to be removed in a future version. The user is suggested to install
/// the corresponding provider and replace the original usage with the one in the provider.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:56 on 2025-04-08 18:47_

```suggestion
                         It still works in Airflow 3.0 but is expected to be removed in a future version."
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:96 on 2025-04-08 18:50_

`expr` and `ranged` refer to the same value, so I think you can avoid `ranged @` and just use `expr.range()` in the call.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR312_AIR312.py.snap`:15 on 2025-04-08 18:52_

```suggestion
AIR312.py:33:1: AIR312 `airflow.hooks.package_index.PackageIndexHook` is deprecated and moved into the `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in future version.
```

I think this sounds better in this case, but it may not work for all of the provider names. Feel free to leave it out if not.

---

_@ntBre approved on 2025-04-08 18:54_

Thanks! This looks good to me, just a few minor nits, mostly in the docs.

---

_@Lee-W reviewed on 2025-04-09 01:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:754 on 2025-04-09 01:53_

Will do! Thanks 🙏

---

_@Lee-W reviewed on 2025-04-09 01:55_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR312_AIR312.py.snap`:15 on 2025-04-09 01:55_

Looks good 🙂 let me update it

---

_@Lee-W reviewed on 2025-04-09 02:19_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:754 on 2025-04-09 02:19_

just updated

---

_@Lee-W reviewed on 2025-04-09 02:31_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:96 on 2025-04-09 02:31_

Good catch! Just updated!

---

_@ntBre approved on 2025-04-09 14:41_

---

_Renamed from "[airflow] Extract AIR312 from AIR302 rules (AIR302, AIR312)" to "[`airflow`] Extract `AIR312` from `AIR302` rules (`AIR302`, `AIR312`)" by @ntBre on 2025-04-09 14:42_

---

_Label `rule` added by @ntBre on 2025-04-09 14:42_

---

_Label `preview` added by @ntBre on 2025-04-09 14:42_

---

_Merged by @ntBre on 2025-04-09 14:43_

---

_Closed by @ntBre on 2025-04-09 14:43_

---
