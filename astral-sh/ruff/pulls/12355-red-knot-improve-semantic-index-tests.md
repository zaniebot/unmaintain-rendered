```yaml
number: 12355
title: "[red-knot] improve semantic index tests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/tests
created_at: 2024-07-17T00:35:08Z
updated_at: 2024-07-17T06:46:52Z
url: https://github.com/astral-sh/ruff/pull/12355
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] improve semantic index tests

---

_@carljm_

Improve semantic index tests with better assertions than just `.len()`, and re-add use-definition test that was commented out in the switch to Salsa initially.


---

_Review requested from @MichaReiser by @carljm on 2024-07-17 00:35_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-17 00:35_

---

_Comment by @github-actions[bot] on 2024-07-17 00:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-07-17 06:20_

---

_@MichaReiser approved on 2024-07-17 06:21_

Thanks

---

_@MichaReiser reviewed on 2024-07-17 06:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:652 on 2024-07-17 06:23_

Totaly unrelated question. Will we have an `Unreachable` type that marks any code branch that can never be reached or is this the `Never` type? I'm asking because I wonder how we would implement the `unreachable-code` rule, now that we no longer build a full CFG.

---

_@carljm reviewed on 2024-07-17 06:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:652 on 2024-07-17 06:46_

That's a good question. If we do resolve an expression type to `Never`, that means the expression is not reachable. But that will only happen if there is a symbol referred to in the expression that gets narrowed to `Never` by the type-narrowing constraints on its definitions. (For example, the expression `1` won't ever be inferred as `Never`, even inside a dead code block, it'll always be inferred as `Literal[1]`. But the expression `x` will infer as `Never` if, for example, the visible definition set it to `False` and then we are inside an `if x:` block.)

To more generally detect all unreachable blocks, we just have to add a bit of code in `TypeInferenceBuilder` so when we hit branch statements in scope-level inference, we infer the test expression, and if it's guaranteed false or true based on the type info we have, we can then mark the appropriate branch as dead code.

---

_Merged by @carljm on 2024-07-17 06:46_

---

_Closed by @carljm on 2024-07-17 06:46_

---

_Branch deleted on 2024-07-17 06:46_

---
