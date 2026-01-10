```yaml
number: 11754
title: "Red-knot: Track scopes per expression"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-expression-scopes
created_at: 2024-06-05T13:50:52Z
updated_at: 2024-06-05T15:53:27Z
url: https://github.com/astral-sh/ruff/pull/11754
synced_at: 2026-01-10T21:56:00Z
```

# Red-knot: Track scopes per expression

---

_Pull request opened by @MichaReiser on 2024-06-05 13:50_

## Summary

This PR extends the symbol table to track for every expression to which scope it belongs. 

To avoid having two `Expression -> X` hash maps, this PR indexes a single `Expr -> ExpressionId` hash map and uses an `IndexVec` for tracking the flow nodes and scopes. 

Open questions:

* It's unclear if we only want this functionality for expressions or all nodes
* Storing the scope for every expression seems a lot (it's small, but still). An alternative design is to instead build up an ancestor tree and iterate over the ancestor tree until we find the first `Function`, `Class`, `Comprehension`, `Lambda` and then resolve the scope using `scopes_by_node`. 

## Test Plan

Added test


---

_Review requested from @carljm by @MichaReiser on 2024-06-05 13:50_

---

_Label `red-knot` added by @MichaReiser on 2024-06-05 13:50_

---

_Comment by @github-actions[bot] on 2024-06-05 14:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-06-05 14:18_

Nice!

---

_Merged by @MichaReiser on 2024-06-05 15:53_

---

_Closed by @MichaReiser on 2024-06-05 15:53_

---

_Branch deleted on 2024-06-05 15:53_

---
