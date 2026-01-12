```yaml
number: 13880
title: "[red-knot] Treat empty intersection as 'object', fix intersection simplification"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-13870
created_at: 2024-10-22T14:16:44Z
updated_at: 2024-10-22T19:02:47Z
url: https://github.com/astral-sh/ruff/pull/13880
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Treat empty intersection as 'object', fix intersection simplification

---

_@sharkdp_

## Summary

- Properly treat the empty intersection as being of type `object`.
- Consequently, change the simplification method to explicitly add `Never` to the positive side of the intersection when collapsing a type such as `int & str` to `Never`, as opposed to just clearing both the positive and the negative side.
- Minor code improvement in `bindings_ty`: use `peekable()` to check whether the iterator over constraints is empty, instead of handling first and subsequent elements separately.

fixes #13870

## Test Plan

- New unit tests for `IntersectionBuilder` to make sure the empty intersection represents `object`.
- Markdown-based regression test for the original issue in #13870

---

_Label `red-knot` added by @sharkdp on 2024-10-22 14:16_

---

_Review requested from @carljm by @sharkdp on 2024-10-22 14:16_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-22 14:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-22 14:16_

---

_@sharkdp reviewed on 2024-10-22 14:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:25 on 2024-10-22 14:23_

Without the fix in this PR, I would previously get
```suggestion
if x != 1:
    reveal_type(x)  # revealed: Literal[2, 3]
    if x != 2:
        reveal_type(x)  # revealed: ~Literal[2] | Literal[3]
```
Note how the bug only shows up if you add more than one constraint.

---

_@sharkdp reviewed on 2024-10-22 14:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1059 on 2024-10-22 14:27_

You might be thinking: isn't this the exact same thing as before? It's not though. We now call `.build()` for every single constraint. Before:
```rs
IntersectionBuilder::new(…)
  .add_positive(bindings_ty)
  .add_positive(constraint_1)
  .add_positive(constraint_2)
  .build();
```
After:
```rs
IntersectionBuilder::new(…)
  .add_positive(
    IntersectionBuilder::new(…)
      .add_positive(bindings_ty)
      .add_positive(constraint_1)
      .build()
  ).add_positive(constraint_2)
  .build()
```


---

_@sharkdp reviewed on 2024-10-22 14:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2115 on 2024-10-22 14:28_

This tests more or less the same thing like the Markdown-based test, only in a much more direct way. Let me know if I should remove it.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1059 on 2024-10-22 14:36_

Hmm... I'm not sure repeatedly calling `build()` is optimal here. `IntersectionBuilder::build()` calls `InnerIntersectionBuilder::build`, and `InnerIntersectionBuilder::build` has these `shrink_to_fit()` calls. I think it only makes sense to do those when the intersection is "finished", rather than every time we add a positive or negative element to the intersection:

https://github.com/astral-sh/ruff/blob/7dbd8f0f8e725d05cc1d15e6ef1c6cc2ed2b2090/crates/red_knot_python_semantic/src/types/builder.rs#L357-L358

What if we moved the simplifications done by `build()` out into a common method that is called by `build`, `add_positive` and `add_negative`? Would that remove the need to call `build()` every time we added a new element to the intersection?

---

_@AlexWaygood reviewed on 2024-10-22 14:36_

---

_Comment by @github-actions[bot] on 2024-10-22 14:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-10-22 16:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:162 on 2024-10-22 16:01_

This is just an unrelated code improvement. No functional change. Let me know if you prefer the old version.

---

_@carljm approved on 2024-10-22 16:02_

Code changes in the updated version look great, thank you! Can you update the PR description (and thus also the merged commit message) to reflect the updated direction? Then feel free to merge.

---

_@carljm reviewed on 2024-10-22 16:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:162 on 2024-10-22 16:03_

I think the new version is nicer, thank you. Don't know if there is any performance difference, but I'm comfortable leaving that question to experimentation if/when this code is identified as a hot spot in profiling.

---

_Renamed from "[red-knot] Constrained types using proper intersections" to "[red-knot] Treat empty intersection as 'object', fix intersection simplification" by @sharkdp on 2024-10-22 19:00_

---

_Merged by @sharkdp on 2024-10-22 19:02_

---

_Closed by @sharkdp on 2024-10-22 19:02_

---

_Branch deleted on 2024-10-22 19:02_

---
