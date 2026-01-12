```yaml
number: 14804
title: "[airflow]: extend removed names (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-air-302-removed-names
created_at: 2024-12-06T04:30:45Z
updated_at: 2024-12-18T00:07:18Z
url: https://github.com/astral-sh/ruff/pull/14804
synced_at: 2026-01-12T15:55:49Z
```

# [airflow]: extend removed names (AIR302)

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

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect. This PR deprecates the following names.
The full list of rules we will extend https://github.com/apache/airflow/issues/44556


#### package
* `airflow.contrib.*` 

#### module
* `airflow.operators.subdag.*` 

#### class
* `airflow.sensors.external_task.ExternalTaskSensorLink` → `airflow.sensors.external_task.ExternalDagLin` 
* `airflow.operators.bash_operator.BashOperator` → `airflow.operators.bash.BashOperator` 
* `airflow.operators.branch_operator.BaseBranchOperator` → `airflow.operators.branch.BaseBranchOperator` 
* `airflow.operators.dummy.EmptyOperator` → `airflow.operators.empty.EmptyOperator` 
* `airflow.operators.dummy.DummyOperator` → `airflow.operators.empty.EmptyOperator` 
* `airflow.operators.dummy_operator.EmptyOperator` → `airflow.operators.empty.EmptyOperator` 
* `airflow.operators.dummy_operator.DummyOperator` → `airflow.operators.empty.EmptyOperator` 
* `airflow.operators.email_operator.EmailOperator` → `airflow.operators.email.EmailOperator` 
* `airflow.sensors.base_sensor_operator.BaseSensorOperator` → `airflow.sensors.base.BaseSensorOperator` 
* `airflow.sensors.date_time_sensor.DateTimeSensor` → `airflow.sensors.date_time.DateTimeSensor` 
* `airflow.sensors.external_task_sensor.ExternalTaskMarker` → `airflow.sensors.external_task.ExternalTaskMarker` 
* `airflow.sensors.external_task_sensor.ExternalTaskSensor` → `airflow.sensors.external_task.ExternalTaskSensor` 
* `airflow.sensors.external_task_sensor.ExternalTaskSensorLink` → `airflow.sensors.external_task.ExternalTaskSensorLink` 
* `airflow.sensors.time_delta_sensor.TimeDeltaSensor` → `airflow.sensors.time_delta.TimeDeltaSensor` 

#### function
* `airflow.utils.decorators.apply_defaults`
* `airflow.www.utils.get_sensitive_variables_fields` → `airflow.utils.log.secrets_masker.get_sensitive_variables_fields` 
* `airflow.www.utils.should_hide_value_for_key` → `airflow.utils.log.secrets_masker.should_hide_value_for_key` 
* `airflow.configuration.get` → `airflow.configuration.conf.get` 
* `airflow.configuration.getboolean` → `airflow.configuration.conf.getboolean` 
* `airflow.configuration.getfloat` → `airflow.configuration.conf.getfloat` 
* `airflow.configuration.getint` → `airflow.configuration.conf.getint` 
* `airflow.configuration.has_option` → `airflow.configuration.conf.has_option` 
* `airflow.configuration.remove_option` → `airflow.configuration.conf.remove_option` 
* `airflow.configuration.as_dict` → `airflow.configuration.conf.as_dict` 
* `airflow.configuration.set` → `airflow.configuration.conf.set` 
* `airflow.secrets.local_filesystem.load_connections` → `airflow.secrets.local_filesystem.load_connections_dict` 
* `airflow.secrets.local_filesystem.get_connection` → `airflow.secrets.local_filesystem.load_connections_dict` 
* `airflow.utils.helpers.chain` → `airflow.models.baseoperator.chain` 
* `airflow.utils.helpers.cross_downstream` → `airflow.models.baseoperator.cross_downstream` 

#### attribute
* in `airflow.utils.trigger_rule.TriggerRule`
    * `DUMMY` 
    * `NONE_FAILED_OR_SKIPPED` 

#### constant / variable
* `airflow.PY\d\d` 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-06 04:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/450132bc859ad4ec1686d4e521efc1efe79a47b8/dev/perf/dags/perf_dag_1.py#L41'>dev/perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0; use `airflow.operators.bash.BashOperator` instead
+ <a href='https://github.com/apache/airflow/blob/450132bc859ad4ec1686d4e521efc1efe79a47b8/dev/perf/dags/perf_dag_1.py#L48'>dev/perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0; use `airflow.operators.bash.BashOperator` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "Extend AIR 302 removed names" to "[airflow]: extend removed names (AIR302)" by @Lee-W on 2024-12-06 09:13_

---

_Marked ready for review by @Lee-W on 2024-12-06 09:16_

---

_@MichaReiser approved on 2024-12-06 10:34_

Nice

---

_Label `rule` added by @MichaReiser on 2024-12-06 10:34_

---

_Label `preview` added by @MichaReiser on 2024-12-06 10:34_

---

_Merged by @MichaReiser on 2024-12-06 10:34_

---

_Closed by @MichaReiser on 2024-12-06 10:34_

---

_Branch deleted on 2024-12-18 00:07_

---
