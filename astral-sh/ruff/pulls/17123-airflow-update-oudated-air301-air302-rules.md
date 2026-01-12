```yaml
number: 17123
title: "[`airflow`] Update oudated `AIR301`, `AIR302` rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: update-AIR302-303-rules
created_at: 2025-04-01T15:05:05Z
updated_at: 2025-04-07T13:45:57Z
url: https://github.com/astral-sh/ruff/pull/17123
synced_at: 2026-01-12T15:56:00Z
```

# [`airflow`] Update oudated `AIR301`, `AIR302` rules

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

Some of the migration rules has been changed during Airflow 3 development. The following are new AIR302 rules. Corresponding AIR301 has also been removed.

* airflow.sensors.external_task_sensor.ExternalTaskMarker → airflow.providers.standard.sensors.external_task.ExternalTaskMarker
* airflow.sensors.external_task_sensor.ExternalTaskSensor → airflow.providers.standard.sensors.external_task.ExternalTaskSensor
* airflow.sensors.external_task_sensor.ExternalTaskSensorLink → airflow.providers.standard.sensors.external_task.ExternalTaskSensorLink
* airflow.sensors.time_delta_sensor.TimeDeltaSensor → airflow.providers.standard.sensors.time_delta.TimeDeltaSensor
* airflow.operators.dagrun_operator.TriggerDagRunLink → airflow.providers.standard.operators.trigger_dagrun.TriggerDagRunLink
* airflow.operators.dagrun_operator.TriggerDagRunOperator → airflow.providers.standard.operators.trigger_dagrun.TriggerDagRunOperator
* airflow.operators.python_operator.BranchPythonOperator → airflow.providers.standard.operators.python.BranchPythonOperator
* airflow.operators.python_operator.PythonOperator → airflow.providers.standard.operators.python.PythonOperator
* airflow.operators.python_operator.PythonVirtualenvOperator → airflow.providers.standard.operators.python.PythonVirtualenvOperator
* airflow.operators.python_operator.ShortCircuitOperator → airflow.providers.standard.operators.python.ShortCircuitOperator
* airflow.operators.latest_only_operator.LatestOnlyOperator → airflow.providers.standard.operators.latest_only.LatestOnlyOperator
* airflow.sensors.date_time_sensor.DateTimeSensor → airflow.providers.standard.sensors.DateTimeSensor
* airflow.operators.email_operator.EmailOperator → airflow.providers.smtp.operators.smtp.EmailOperator
* airflow.operators.email.EmailOperator → airflow.providers.smtp.operators.smtp.EmailOperator
* airflow.operators.bash.BashOperator → airflow.providers.standard.operators.bash.BashOperator
* airflow.operators.EmptyOperator → airflow.providers.standard.operators.empty.EmptyOperator

closes: https://github.com/astral-sh/ruff/issues/17103

## Test Plan

<!-- How was it tested? -->

The test fixture has been updated and checked after each change and later reorganized in the latest commit


---

_Converted to draft by @Lee-W on 2025-04-01 15:05_

---

_Comment by @github-actions[bot] on 2025-04-01 15:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/airflow-core/tests/unit/utils/test_dot_renderer.py#L103'>airflow-core/tests/unit/utils/test_dot_renderer.py:103:22:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/airflow-core/tests/unit/utils/test_dot_renderer.py#L83'>airflow-core/tests/unit/utils/test_dot_renderer.py:83:22:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR302 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR302 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/performance/src/performance_dags/performance_dag/performance_dag.py#L111'>performance/src/performance_dags/performance_dag/performance_dag.py:111:13:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/8cba39a954265977e37eff76d2e774632c65066d/providers/ydb/tests/system/ydb/example_ydb.py#L111'>providers/ydb/tests/system/ydb/example_ydb.py:111:23:</a> AIR302 `airflow.operators.python.PythonOperator` is moved into `standard` provider in Airflow 3.0;
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 8 | 6 | 2 | 0 | 0 |

</p>
</details>




---

_Renamed from "Update air302 303 rules" to "[airflow] update oudated AIR302, AIR303 rules" by @Lee-W on 2025-04-01 15:13_

---

_Renamed from "[airflow] update oudated AIR302, AIR303 rules" to "[airflow] update oudated AIR301, AIR302 rules" by @Lee-W on 2025-04-03 02:59_

---

_Marked ready for review by @Lee-W on 2025-04-03 02:59_

---

_@Lee-W reviewed on 2025-04-03 02:59_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:174 on 2025-04-03 02:59_

This is a fixed found during this change

---

_Assigned to @ntBre by @ntBre on 2025-04-03 22:13_

---

_Comment by @Lee-W on 2025-04-04 12:21_

@ntBre Would be nice if we can get a quick review on this one 🙂  Thanks 🙏

---

_Comment by @ntBre on 2025-04-04 12:53_

Will do! Feel free to ping me or request my review on any of these. There's a chance I could have missed them before taking over from Dhruv. I assigned myself to a few last night :)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:913 on 2025-04-04 16:28_

Does this need to be `rest @ ..`? It looks like each of the patterns are just one additional part, so I think it could just be `rest` and remove the `[]` braces from the nested `match` patterns.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:936 on 2025-04-04 16:28_

Same as above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:239 on 2025-04-04 16:29_

Is this a TODO for later, or should it be handled here?

---

_@ntBre approved on 2025-04-04 19:27_

Thanks! Just a few nits/questions

---

_@Lee-W reviewed on 2025-04-07 01:44_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:913 on 2025-04-07 01:44_

Good catch! Just updated it. Thank!

---

_@Lee-W reviewed on 2025-04-07 01:44_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:936 on 2025-04-07 01:44_

Good catch! Just updated it. Thank!

---

_@Lee-W reviewed on 2025-04-07 01:45_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:239 on 2025-04-07 01:45_

It's actually going to be moved to 312. It was somewhat like a reminder for myself to handle it. but we already have to full list wait for implementing here https://github.com/apache/airflow/issues/48560#issuecomment-2772608006

let me remove this todo

---

_Review requested from @ntBre by @Lee-W on 2025-04-07 01:47_

---

_Comment by @Lee-W on 2025-04-07 01:47_

I think this one is ready again. Thanks!

---

_Label `rule` added by @MichaReiser on 2025-04-07 06:31_

---

_Label `preview` added by @MichaReiser on 2025-04-07 06:31_

---

_Renamed from "[airflow] update oudated AIR301, AIR302 rules" to "[`airflow`] Update oudated `AIR301`, `AIR302` rules" by @ntBre on 2025-04-07 13:45_

---

_@ntBre approved on 2025-04-07 13:45_

Thanks!

---

_Merged by @ntBre on 2025-04-07 13:45_

---

_Closed by @ntBre on 2025-04-07 13:45_

---
