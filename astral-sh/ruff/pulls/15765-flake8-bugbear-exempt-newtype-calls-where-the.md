```yaml
number: 15765
title: "[`flake8-bugbear`] Exempt `NewType` calls where the original type is immutable (`B008`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: B008
created_at: 2025-01-27T07:44:05Z
updated_at: 2025-01-29T12:35:32Z
url: https://github.com/astral-sh/ruff/pull/15765
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-bugbear`] Exempt `NewType` calls where the original type is immutable (`B008`)

---

_@InSyncWithFoo_

## Summary

Resolves #12717.

This change incorporates the logic added in #15588.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-27 07:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-01-27 10:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:162 on 2025-01-27 10:58_

I think we should move this function to https://github.com/astral-sh/ruff/blob/2ef94e5f3e6e57a19151334b598e2bf381e97dac/crates/ruff_python_semantic/src/analyze/typing.rs as it's being used by rules from multiple plugins, is related to the `typing` module. We should also add docs to it.

---

_Label `rule` added by @dhruvmanila on 2025-01-27 10:59_

---

_@dhruvmanila reviewed on 2025-01-27 11:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:162 on 2025-01-27 11:00_

We should also update the rule documentation to specify this behavior regarding `NewType`.

---

_@dhruvmanila approved on 2025-01-29 10:21_

Thanks.

I added docs for the `is_immutable_newtype_call` and updated the parameter to use `ExprName` instead of `Expr`

---

_Merged by @dhruvmanila on 2025-01-29 10:26_

---

_Closed by @dhruvmanila on 2025-01-29 10:26_

---

_Branch deleted on 2025-01-29 12:35_

---
