```yaml
number: 13594
title: "[red-knot] [minor] Improve helper methods for builtin types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-helpers
created_at: 2024-10-01T17:03:16Z
updated_at: 2024-10-01T17:38:35Z
url: https://github.com/astral-sh/ruff/pull/13594
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] [minor] Improve helper methods for builtin types

---

_Pull request opened by @AlexWaygood on 2024-10-01 17:03_

## Summary

https://github.com/astral-sh/ruff/commit/16394880822f017ac3c5856f6c29cfa95f779c97 added a `Type::builtin_str()` helper method. We call `builtin_symbol_ty(&db, "int")` at least as often, so I think it probably makes sense to have a helper method for that as well. In the vast majority of cases, we're also immediately calling `.to_instance()` on the returned type, so I think it makes sense to have `builtin_str_instance()` and `builtin_int_instance()` helpers rather than `builtin_str()` and `builtin_int` helpers.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-01 17:03_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-01 17:03_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-01 17:03_

---

_Comment by @github-actions[bot] on 2024-10-01 17:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-10-01 17:37_

---

_Merged by @AlexWaygood on 2024-10-01 17:38_

---

_Closed by @AlexWaygood on 2024-10-01 17:38_

---

_Branch deleted on 2024-10-01 17:38_

---
