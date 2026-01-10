```yaml
number: 13318
title: "[red-knot] Add control flow for `for` loops"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/for-loop-flow
created_at: 2024-09-10T20:57:59Z
updated_at: 2024-09-10T22:14:00Z
url: https://github.com/astral-sh/ruff/pull/13318
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add control flow for `for` loops

---

_Pull request opened by @AlexWaygood on 2024-09-10 20:57_

## Summary

Same as #12413, but for `for` loops!

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-10 20:57_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-10 20:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-10 20:57_

---

_Comment by @github-actions[bot] on 2024-09-10 21:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-10 21:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:647 on 2024-09-10 21:52_

We should have a TODO here for the actual loopy part of flow control: the fact that definitions in the loop body... are visible to the loop body. But we don't want to implement this until we have fixpoint iteration support in Salsa, or else we'll just panic a lot with cycle problems.

I think I also should have added such a TODO to the `while` control flow, and didn't. But I guess that could also be added in this PR?

---

_@carljm approved on 2024-09-10 21:52_

Very nice!

---

_Merged by @AlexWaygood on 2024-09-10 22:04_

---

_Closed by @AlexWaygood on 2024-09-10 22:04_

---

_Branch deleted on 2024-09-10 22:04_

---
