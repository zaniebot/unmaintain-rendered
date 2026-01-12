```yaml
number: 16635
title: "[`flake8-datetimez`] Stabilize `datetime-min-max` (`DTZ901`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/dtz901-0.10
created_at: 2025-03-11T16:33:01Z
updated_at: 2025-03-12T18:43:36Z
url: https://github.com/astral-sh/ruff/pull/16635
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-datetimez`] Stabilize `datetime-min-max` (`DTZ901`)

---

_@ntBre_

Summary
--

Stabilizes DTZ901, renames the rule function to match the rule name, removes the `preview_rules` test, and handles some nits in the docs (mention `min` first to match the rule name too).

Test Plan
--

1 closed issue on 2024-11-12, 4 days after the rule was added. No issues since


---

_Label `rule` added by @ntBre on 2025-03-11 16:33_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 16:33_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 16:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fdtz901-0.10)

### Merging #16635 will **degrade performances by 12.88%**

<sub>Comparing <code>brent/dtz901-0.10</code> (5cae870) with <code>micha/ruff-0.10</code> (5ec7c13)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fdtz901-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.88% |


---

_Comment by @github-actions[bot] on 2025-03-11 16:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+11 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/airflow/example_dags/example_inlet_event_extra.py#L38'>airflow/example_dags/example_inlet_event_extra.py:38:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/airflow/example_dags/example_inlet_event_extra.py#L53'>airflow/example_dags/example_inlet_event_extra.py:53:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/airflow/example_dags/example_outlet_event_extra.py#L39'>airflow/example_dags/example_outlet_event_extra.py:39:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/airflow/example_dags/example_outlet_event_extra.py#L53'>airflow/example_dags/example_outlet_event_extra.py:53:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/airflow/example_dags/example_outlet_event_extra.py#L67'>airflow/example_dags/example_outlet_event_extra.py:67:16:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/providers/google/src/airflow/providers/google/cloud/operators/gcs.py#L805'>providers/google/src/airflow/providers/google/cloud/operators/gcs.py:805:46:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/providers/google/src/airflow/providers/google/cloud/transfers/mysql_to_gcs.py#L137'>providers/google/src/airflow/providers/google/cloud/transfers/mysql_to_gcs.py:137:26:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/cli/commands/remote_commands/test_dag_command.py#L351'>tests/cli/commands/remote_commands/test_dag_command.py:351:37:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/dag_processing/test_manager.py#L495'>tests/dag_processing/test_manager.py:495:75:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/dag_processing/test_manager.py#L513'>tests/dag_processing/test_manager.py:513:75:</a> DTZ901 Use of `datetime.datetime.max` without timezone information
+ <a href='https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/models/test_dag.py#L2442'>tests/models/test_dag.py:2442:24:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ901 | 11 | 11 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 16:51_

I think the ecosystem check revealed three false positives:

https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/cli/commands/remote_commands/test_dag_command.py#L350-L351

https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/dag_processing/test_manager.py#L494-L496

https://github.com/apache/airflow/blob/eebdf4e9fbbe9badd1f93dbaf1c9a0d1d2aa357c/tests/dag_processing/test_manager.py#L512-L514

I don't think there's much we can do here because `timezone.make_aware` is defined in the `airflow.utils` module, but I think it's a false positive because it clearly sounds like `make_aware` is addressing the fact that "datetime.max and datetime.min are non-timezone-aware datetime objects" from our docs. We'd probably need some kind of configuration allowlist to handle that.

---

_Comment by @UnknownPlatypus on 2025-03-12 06:42_

Django also have a [`timezone.make_aware`](https://docs.djangoproject.com/en/5.1/ref/utils/#django.utils.timezone.make_aware) util so you might encounter false positive in django code bases too. 

You could maybe let the rule pass if it's wrapped in any `timezone.make_aware` call ?

---

_@MichaReiser approved on 2025-03-12 08:18_

Airflow would have to use a noqa comment in this case which is an explicit acknowledgement that they handled the no-timezone case. 

This is also not ap roblem specific to `DTZ901` (as far as I understand). E.g. https://docs.astral.sh/ruff/rules/call-date-today/ has the same problem that we flag 

```py
import datetime

timezone.make_aware(datetime.datetime.today())
```

---

_Comment by @ntBre on 2025-03-12 18:42_

Sounds good, moving forward with stabilization.

---

_Merged by @ntBre on 2025-03-12 18:43_

---

_Closed by @ntBre on 2025-03-12 18:43_

---

_Branch deleted on 2025-03-12 18:43_

---
