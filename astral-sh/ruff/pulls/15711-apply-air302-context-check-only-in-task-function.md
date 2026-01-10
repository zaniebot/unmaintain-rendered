```yaml
number: 15711
title: "Apply `AIR302`-context check only in `@task` function"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/airflow-refactor
created_at: 2025-01-24T07:06:59Z
updated_at: 2025-01-24T10:47:36Z
url: https://github.com/astral-sh/ruff/pull/15711
synced_at: 2026-01-10T19:57:22Z
```

# Apply `AIR302`-context check only in `@task` function

---

_Pull request opened by @dhruvmanila on 2025-01-24 07:06_

This PR updates `AIR302` to only apply the context keys check in `@task` decorated function.

Ref: https://github.com/astral-sh/ruff/pull/15144

---

_Label `internal` added by @dhruvmanila on 2025-01-24 07:06_

---

_Comment by @dhruvmanila on 2025-01-24 07:07_

cc @sunank200 @Lee-W 

---

_@dhruvmanila reviewed on 2025-01-24 07:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:436 on 2025-01-24 07:11_

Just want to make sure that this is correct before merging - the update here is that the `**` parameter isn't limited to `context` and `kwargs`, so the following is valid as well:

```python
@task 
def some_task(**variables):
	variables.get('conf')
```

Is this correct? Or, do we need to limit it to `context` and `kwargs`?

---

_Comment by @github-actions[bot] on 2025-01-24 07:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Lee-W reviewed on 2025-01-24 07:15_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:436 on 2025-01-24 07:15_

ah, let me try with `variables` again. It reminds me `kwargs` is a convention but not a keyword 🤦‍♂️

---

_@dhruvmanila reviewed on 2025-01-24 07:22_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:436 on 2025-01-24 07:22_

Got it, thanks

---

_@Lee-W reviewed on 2025-01-24 07:23_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:436 on 2025-01-24 07:23_

right, anything starts with `**` should be caught

---

_Merged by @dhruvmanila on 2025-01-24 07:30_

---

_Closed by @dhruvmanila on 2025-01-24 07:30_

---

_Branch deleted on 2025-01-24 07:30_

---

_Comment by @Lee-W on 2025-01-24 10:47_

HI @dhruvmanila , https://github.com/astral-sh/ruff/pull/15713 this should fix part of the issues.

---
