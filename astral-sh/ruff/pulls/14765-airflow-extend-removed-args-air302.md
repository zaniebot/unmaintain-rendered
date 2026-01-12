```yaml
number: 14765
title: "[airflow]: extend removed args (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-removed-args
created_at: 2024-12-04T06:56:33Z
updated_at: 2024-12-18T00:07:20Z
url: https://github.com/astral-sh/ruff/pull/14765
synced_at: 2026-01-12T15:55:48Z
```

# [airflow]: extend removed args (AIR302)

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

* in `DAG`
    * `sla_miss_callback` was removed
* in `airflow.operators.trigger_dagrun.TriggerDagRunOperator`
    * `execution_date` was removed
* in `airflow.operators.weekday.DayOfWeekSensor`, `airflow.operators.datetime.BranchDateTimeOperator` and `airflow.operators.weekday.BranchDayOfWeekOperator`
    * `use_task_execution_day` was removed in favor of  `use_task_logical_date`

The full list of rules we will extend https://github.com/apache/airflow/issues/44556

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.

---

_Comment by @github-actions[bot] on 2024-12-04 07:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "feat(AIR302): extend removed args" to "[airflow]: extend removed args (AIR302)" by @Lee-W on 2024-12-04 09:13_

---

_Marked ready for review by @Lee-W on 2024-12-04 14:43_

---

_Comment by @Lee-W on 2024-12-06 15:24_

@MichaReiser would be nice if you could also take a look at this one when you have some time 🙂  many thanks! 🙏

---

_@MichaReiser approved on 2024-12-06 15:59_

Sorry. I must have missed this PR. Thanks for the ping. This looks good.

---

_Label `rule` added by @MichaReiser on 2024-12-06 16:00_

---

_Label `preview` added by @MichaReiser on 2024-12-06 16:00_

---

_Merged by @MichaReiser on 2024-12-06 16:00_

---

_Closed by @MichaReiser on 2024-12-06 16:00_

---

_Comment by @Lee-W on 2024-12-07 04:02_

Thanks for your immediate help 🙏

---

_Branch deleted on 2024-12-18 00:07_

---
