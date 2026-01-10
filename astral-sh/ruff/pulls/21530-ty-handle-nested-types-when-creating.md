```yaml
number: 21530
title: "[ty] Handle nested types when creating specializations from constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/specialize-cycles
created_at: 2025-11-19T19:56:19Z
updated_at: 2025-11-19T22:37:17Z
url: https://github.com/astral-sh/ruff/pull/21530
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Handle nested types when creating specializations from constraint sets

---

_Pull request opened by @dcreager on 2025-11-19 19:56_

#21414 added the ability to create a specialization from a constraint set. It handled mutually constrained typevars just fine, e.g. given `T ≤ int ∧ U = T` we can infer `T = int, U = int`.

But it didn't handle _nested_ constraints correctly, e.g. `T ≤ int ∧ U = list[T]`. Now we do! This requires doing a fixed-point "apply the specialization to itself" step to propagate the assignments of any nested typevars, and then a cycle detection check to make sure we don't have an infinite expansion in the specialization.

This gets at an interesting nuance in our constraint set structure that @sharkdp has asked about before. Constraint sets are BDDs, and each internal node represents an _individual constraint_, of the form `lower ≤ T ≤ upper`. `lower` and `upper` are allowed to be other typevars, but only if they appear "later" in the arbitary ordering that we establish over typevars. The main purpose of this is to avoid infinite expansion for mutually constrained typevars.

However, that restriction doesn't help us here, because only applies when `lower` and `upper` _are_ typevars, not when they _contain_ typevars. That distinction is important, since it means the restriction does not affect our expressiveness: we can always rewrite `Never ≤ T ≤ U` (a constraint on `T`) into `T ≤ U ≤ object` (a constraint on `U`). The same is not true of `Never ≤ T ≤ list[U]` — there is no "inverse" of `list` that we could apply to both sides to transform this into a constraint on a bare `U`.

---

_Label `internal` added by @dcreager on 2025-11-19 19:56_

---

_Review requested from @carljm by @dcreager on 2025-11-19 19:56_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-19 19:56_

---

_Review requested from @sharkdp by @dcreager on 2025-11-19 19:56_

---

_Label `ty` added by @dcreager on 2025-11-19 19:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 19:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-19 19:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:270 on 2025-11-19 21:32_

Could `finished` be eliminated in favor of removing keys from `reachable_typevars` when finished checking them?

---

_@carljm approved on 2025-11-19 21:33_

---

_@dcreager reviewed on 2025-11-19 22:00_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:270 on 2025-11-19 22:00_

Hmm, yes! Have to jump through some borrow checker hoops but nothing too bad

---

_Merged by @dcreager on 2025-11-19 22:37_

---

_Closed by @dcreager on 2025-11-19 22:37_

---

_Branch deleted on 2025-11-19 22:37_

---
