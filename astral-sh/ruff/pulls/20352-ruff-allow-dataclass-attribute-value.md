```yaml
number: 20352
title: "[`ruff`] Allow dataclass attribute value instantiation from nested frozen dataclass (`RUF009`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/ruff-20266
created_at: 2025-09-11T17:31:07Z
updated_at: 2025-09-12T20:46:50Z
url: https://github.com/astral-sh/ruff/pull/20352
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Allow dataclass attribute value instantiation from nested frozen dataclass (`RUF009`)

---

_Pull request opened by @IDrokin117 on 2025-09-11 17:31_

## Summary
Resolves #20266

Definition of the frozen dataclass attribute can be instantiation of a nested frozen dataclass as well as a non-nested one.

### Problem explanation
The `function_call_in_dataclass_default` function is invoked during the "defined scope" stage, after all scopes have been processed. At this point, the semantic references the top-level scope. When `SemanticModel::lookup_attribute` executes, it searches for bindings in the top-level module scope rather than the class scope, resulting in an error.

To solve this issue, the lookup should be evaluated through the class scope.

## Test Plan
- Added test case from issue 


---

_Comment by @github-actions[bot] on 2025-09-11 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-09-12 20:24_

---

_@ntBre approved on 2025-09-12 20:46_

Thank you! This makes sense to me

---

_Merged by @ntBre on 2025-09-12 20:46_

---

_Closed by @ntBre on 2025-09-12 20:46_

---
