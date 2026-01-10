```yaml
number: 14288
title: "[`flake8-datetimez`] Usages of `datetime.max`/`datetime.min` (`DTZ901`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: DTZ901
created_at: 2024-11-12T00:06:39Z
updated_at: 2024-11-12T21:09:30Z
url: https://github.com/astral-sh/ruff/pull/14288
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-datetimez`] Usages of `datetime.max`/`datetime.min` (`DTZ901`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-12 00:06_

## Summary

Resolves #13217.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-12 00:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/airflow/example_dags/example_inlet_event_extra.py#L38'>airflow/example_dags/example_inlet_event_extra.py:38:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/airflow/example_dags/example_inlet_event_extra.py#L53'>airflow/example_dags/example_inlet_event_extra.py:53:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/airflow/example_dags/example_outlet_event_extra.py#L39'>airflow/example_dags/example_outlet_event_extra.py:39:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/airflow/example_dags/example_outlet_event_extra.py#L53'>airflow/example_dags/example_outlet_event_extra.py:53:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/airflow/example_dags/example_outlet_event_extra.py#L67'>airflow/example_dags/example_outlet_event_extra.py:67:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/providers/src/airflow/providers/google/cloud/operators/gcs.py#L807'>providers/src/airflow/providers/google/cloud/operators/gcs.py:807:46:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/providers/src/airflow/providers/google/cloud/operators/gcs.py#L810'>providers/src/airflow/providers/google/cloud/operators/gcs.py:810:46:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/providers/src/airflow/providers/google/cloud/transfers/mysql_to_gcs.py#L129'>providers/src/airflow/providers/google/cloud/transfers/mysql_to_gcs.py:129:26:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/tests/cli/commands/test_dag_command.py#L383'>tests/cli/commands/test_dag_command.py:383:37:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/tests/dag_processing/test_job_runner.py#L773'>tests/dag_processing/test_job_runner.py:773:53:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/tests/dag_processing/test_job_runner.py#L804'>tests/dag_processing/test_job_runner.py:804:53:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/f924ecb6e3d6e85a83b261cc7adc3a3c05c2b1ee/tests/models/test_dag.py#L2451'>tests/models/test_dag.py:2451:24:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L83'>examples/models/daylight.py:83:12:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ901 | 13 | 13 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_datetimez/rules/datetime_max_min.rs`:92 on 2024-11-12 11:13_

Instead of panicking in `MaxMin::from` if `attr` isn't `max` or `min`, change `MaxMin::from` to return an `Option` and use it like this

```suggestion
    let Some(maxmin) = match qualified_name.segments() {
        ["datetime", "datetime", attr] => MaxMin::from(attr),
        _ => None,
    } else {
    	return
  	};
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_datetimez/rules/datetime_max_min.rs`:104 on 2024-11-12 11:14_

```suggestion
pub(super) fn followed_by_replace_tzinfo(semantic: &SemanticModel -> bool {
```

---

_@MichaReiser approved on 2024-11-12 11:17_

Neat

---

_Label `rule` added by @MichaReiser on 2024-11-12 11:17_

---

_Label `preview` added by @MichaReiser on 2024-11-12 11:17_

---

_@MichaReiser reviewed on 2024-11-12 11:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:709 on 2024-11-12 11:18_

I assume the reason for using `9XX` is to make it clear that this isn't an upstream rule?

---

_@InSyncWithFoo reviewed on 2024-11-12 12:05_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/codes.rs`:709 on 2024-11-12 12:05_

Yes, that's correct.

---

_Merged by @charliermarsh on 2024-11-12 20:36_

---

_Closed by @charliermarsh on 2024-11-12 20:36_

---

_Branch deleted on 2024-11-12 21:09_

---
