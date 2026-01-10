```yaml
number: 16014
title: "[`airflow`] Add `external_task.{ExternalTaskMarker, ExternalTaskSensor}` for `AIR302`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: AIR302-refactor
created_at: 2025-02-07T09:48:46Z
updated_at: 2025-03-31T21:56:21Z
url: https://github.com/astral-sh/ruff/pull/16014
synced_at: 2026-01-10T19:40:36Z
```

# [`airflow`] Add `external_task.{ExternalTaskMarker, ExternalTaskSensor}` for `AIR302`

---

_Pull request opened by @Lee-W on 2025-02-07 09:48_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->



Apply suggestions similar to https://github.com/astral-sh/ruff/pull/15922#discussion_r1940697704


## Test Plan

<!-- How was it tested? -->

a test fixture has been updated


---

_Renamed from "refactor(AIR302): group similiar conditions" to "[airflow] refactor: group multiple statement into single statement with or (AIR302)" by @Lee-W on 2025-02-07 09:49_

---

_Comment by @github-actions[bot] on 2025-02-07 09:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-02-07 11:03_

Just a sanity check that the following didn't exists earlier so want to make sure that it's valid:
* [..., `external_task`, `ExternalTaskMarker`]
* [..., `external_task`, `ExternalTaskSensor`]

---

_@dhruvmanila reviewed on 2025-02-07 11:04_

---

_@dhruvmanila reviewed on 2025-02-07 11:05_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-02-07 11:05_

If these are new additions, we can update the PR title to make sure it reflects that, the refactor is fine to include here.

---

_Label `rule` added by @dhruvmanila on 2025-02-07 11:05_

---

_Label `preview` added by @dhruvmanila on 2025-02-07 11:05_

---

_@Lee-W reviewed on 2025-02-07 11:05_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-02-07 11:05_

yes, they're valid

---

_Renamed from "[airflow] refactor: group multiple statement into single statement with or (AIR302)" to "[airflow] include "ExternalTaskMarker" and "ExternalTaskSensor" in the "external_task" module checking   (AIR302)" by @Lee-W on 2025-02-07 11:06_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-02-07 11:07_

Updated. Please check whether it's better. 🙂 

---

_@Lee-W reviewed on 2025-02-07 11:07_

---

_@dhruvmanila approved on 2025-02-07 11:07_

---

_Renamed from "[airflow] include "ExternalTaskMarker" and "ExternalTaskSensor" in the "external_task" module checking   (AIR302)" to "[`airflow`] Add `external_task.{ExternalTaskMarker, ExternalTaskSensor}` for `AIR302`" by @dhruvmanila on 2025-02-07 11:08_

---

_Merged by @dhruvmanila on 2025-02-07 11:08_

---

_Closed by @dhruvmanila on 2025-02-07 11:08_

---

_Comment by @pbhuss on 2025-03-31 21:56_

This looks like a bug:
```
AIR302 `airflow.sensors.external_task.ExternalTaskSensor` is removed in Airflow 3.0
[...]
= help: Use `airflow.sensors.external_task.ExternalTaskSensor` instead
```

It's suggesting replacing with itself

---
