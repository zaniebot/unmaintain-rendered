```yaml
number: 13762
title: "[red-knot] Simplify some branches in `infer_subscript_expression`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-simplify
created_at: 2024-10-15T13:45:17Z
updated_at: 2024-10-16T10:42:03Z
url: https://github.com/astral-sh/ruff/pull/13762
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Simplify some branches in `infer_subscript_expression`

---

_@AlexWaygood_

## Summary

Just a small simplification to remove some unnecessary complexity here. Rather than using separate branches for subscript expressions involving boolean literals, we can simply convert them to integer literals and reuse the logic in the `IntLiteral` branches.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-15 13:45_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-15 13:45_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-15 13:45_

---

_Comment by @github-actions[bot] on 2024-10-15 13:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-10-16 06:07_

This is neat!

---

_Merged by @AlexWaygood on 2024-10-16 06:58_

---

_Closed by @AlexWaygood on 2024-10-16 06:58_

---

_Branch deleted on 2024-10-16 06:58_

---

_Comment by @carljm on 2024-10-16 10:42_

Agreed, very nice!

---
