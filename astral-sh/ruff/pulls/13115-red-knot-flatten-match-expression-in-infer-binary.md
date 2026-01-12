```yaml
number: 13115
title: "red-knot: flatten match expression in `infer_binary_expression`"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-flatten-a-match
created_at: 2024-08-26T19:18:58Z
updated_at: 2024-08-27T23:44:40Z
url: https://github.com/astral-sh/ruff/pull/13115
synced_at: 2026-01-12T15:55:43Z
```

# red-knot: flatten match expression in `infer_binary_expression`

---

_@chriskrycho_

## Summary

This fixes the outstanding TODO and make it easier to work with new cases. (Tidy first, *then* implement, basically!)

## Test Plan

After making this change all the existing tests still pass. A classic refactor win. 🎉 


---

_Review requested from @carljm by @chriskrycho on 2024-08-26 19:18_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-26 19:18_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-26 19:18_

---

_@carljm approved on 2024-08-26 19:32_

Love it!

---

_Comment by @github-actions[bot] on 2024-08-26 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-08-26 19:34_

---

_Closed by @carljm on 2024-08-26 19:34_

---

_Branch deleted on 2024-08-26 21:09_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:44_

---
