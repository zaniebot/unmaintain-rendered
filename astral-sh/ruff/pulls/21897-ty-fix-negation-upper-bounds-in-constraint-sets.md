```yaml
number: 21897
title: "[ty] Fix negation upper bounds in constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/fix-intersection-bug
created_at: 2025-12-10T15:54:07Z
updated_at: 2025-12-10T20:07:52Z
url: https://github.com/astral-sh/ruff/pull/21897
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Fix negation upper bounds in constraint sets

---

_@dcreager_

This fixes the logic error that @sharkdp [found](https://github.com/astral-sh/ruff/pull/21871#discussion_r2605755588) in the constraint set upper bound normalization logic I introduced in #21871.

I had originally claimed that `(T ≤ α & ~β)` should simplify into `(T ≤ α) ∧ ¬(T ≤ β)`. But that also suggests that `T ≤ ~β` should simplify to `¬(T ≤ β)` on its own, and that's not correct.

The correct simplification is that `~α` is an "atomic" type, not an "intersection" for the purposes of our upper bound simplifcation. So `(T ≤ α & ~β)` should simplify to `(T ≤ α) ∧ (T ≤ ~β)`. That is, break apart the elements of a (proper) intersection, regardless of whether each element is negated or not.

This PR fixes the logic, adds a test case, and updates the comments to be hopefully more clear and accurate.

---

_Label `internal` added by @dcreager on 2025-12-10 15:54_

---

_Review requested from @carljm by @dcreager on 2025-12-10 15:54_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-10 15:54_

---

_Review requested from @sharkdp by @dcreager on 2025-12-10 15:54_

---

_Label `ty` added by @dcreager on 2025-12-10 15:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 15:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 15:58_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5258 diagnostics
+ Found 5259 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:216 on 2025-12-10 19:41_

This DSL is so cool

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:539 on 2025-12-10 19:42_

So subtle :smiley: 

---

_@sharkdp approved on 2025-12-10 19:42_

Thank you!

---

_Merged by @dcreager on 2025-12-10 20:07_

---

_Closed by @dcreager on 2025-12-10 20:07_

---

_Branch deleted on 2025-12-10 20:07_

---
