```yaml
number: 14627
title: "Check that `airflow` module is seen for `AIR001`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/check-airflow-module
created_at: 2024-11-27T07:20:20Z
updated_at: 2024-11-27T08:36:30Z
url: https://github.com/astral-sh/ruff/pull/14627
synced_at: 2026-01-12T15:55:48Z
```

# Check that `airflow` module is seen for `AIR001`

---

_@dhruvmanila_

_No description provided._

---

_Label `internal` added by @dhruvmanila on 2024-11-27 07:20_

---

_@dhruvmanila reviewed on 2024-11-27 07:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/task_variable_name.rs`:77 on 2024-11-27 07:21_

cc @uranusjr is the `AIR001` rule valid for any symbols that's being imported from the `airflow` module? As per the rule documentation, it seems to mention specifically "Airflow Operators", so should this be restricted to `airflow.operators.*`?

---

_Merged by @dhruvmanila on 2024-11-27 07:25_

---

_Closed by @dhruvmanila on 2024-11-27 07:25_

---

_Branch deleted on 2024-11-27 07:25_

---

_Comment by @github-actions[bot] on 2024-11-27 07:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@uranusjr reviewed on 2024-11-27 08:36_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/task_variable_name.rs`:77 on 2024-11-27 08:36_

It probably should be restricted to `airflow.operators.*` and `airflow.providers.**.operators.*`.

---
