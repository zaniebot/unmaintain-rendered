```yaml
number: 13319
title: "[red-knot] Fix `.to_instance()` for union types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-to-instance
created_at: 2024-09-10T21:55:59Z
updated_at: 2024-09-10T22:50:38Z
url: https://github.com/astral-sh/ruff/pull/13319
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Fix `.to_instance()` for union types

---

_@AlexWaygood_

_No description provided._

---

_Review requested from @carljm by @AlexWaygood on 2024-09-10 21:56_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-10 21:56_

---

_Label `red-knot` added by @AlexWaygood on 2024-09-10 21:56_

---

_Comment by @github-actions[bot] on 2024-09-10 22:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:458 on 2024-09-10 22:28_

This diagnostic should be handled by just visiting all Name exprs with Load context and checking if their type is unbound, it shouldn't be handled in every method of `Type` where we see an `Unbound`. So I'm not sure we need a TODO for it here.

(TBH I'm getting more and more convinced that `Unbound` should not be a `Type` variant at all.)

---

_@MichaReiser approved on 2024-09-10 22:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:456 on 2024-09-10 22:31_

Yeah - it should just be a map over the elements resulting in a new intersection, as far as positive elements of the intersection go. But negative elements make it a bit tricky. I _think_ maybe they would just get discarded, but I need to think about it more. Doesn't need to be handled in this PR, anyway.

---

_@carljm approved on 2024-09-10 22:33_

Nice.

---

_Merged by @AlexWaygood on 2024-09-10 22:41_

---

_Closed by @AlexWaygood on 2024-09-10 22:41_

---

_Branch deleted on 2024-09-10 22:41_

---
