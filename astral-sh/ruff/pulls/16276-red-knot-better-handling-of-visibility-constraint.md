```yaml
number: 16276
title: "[red-knot] Better handling of visibility constraint copies"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/better-constraint-copies
created_at: 2025-02-20T14:22:11Z
updated_at: 2025-02-21T14:16:29Z
url: https://github.com/astral-sh/ruff/pull/16276
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Better handling of visibility constraint copies

---

_Pull request opened by @dcreager on 2025-02-20 14:22_

Two related changes.  For context:

1. We were maintaining two separate arenas of `Constraint`s in each use-def map.  One was used for narrowing constraints, and the other for visibility constraints.  The visibility constraint arena was interned, ensuring that we always used the same ID for any particular `Constraint`.  The narrowing constraint arena was not interned.

2. The TDD code relies on _all_ TDD nodes being interned and reduced.  This is an important requirement for TDDs to be a canonical form, which allows us to use a single int comparison to test for "always true/false" and to compare two TDDs for equivalence.  But we also need to support an individual `Constraint` having multiple values in a TDD evaluation (e.g. to handle a `while` condition having different values the first time it's evaluated vs later times).  Previously, we handled that by introducing a "copy" number, which was only there as a disambiguator, to allow an interned, deduplicated constraint ID to appear in the TDD formula multiple times.

A better way to handle (2) is to not intern the constraints in the visibility constraint arena!  The caller now gets to decide: if they add a `Constraint` to the arena more than once, they get distinct `ScopedConstraintId`s — which the TDD code will treat as distinct variables, allowing them to take on different values in the ternary function.

With that in place, we can then consolidate on a single (non-interned) arena, which is shared for both narrowing and visibility constraints.


---

_Label `internal` added by @dcreager on 2025-02-20 14:22_

---

_Label `red-knot` added by @dcreager on 2025-02-20 14:22_

---

_Review requested from @carljm by @dcreager on 2025-02-20 14:22_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-20 14:22_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-20 14:22_

---

_Review requested from @sharkdp by @dcreager on 2025-02-20 14:22_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-02-20 14:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:24 on 2025-02-20 22:53_

This comment (the part about "ensuring that we only store any particular constraint once") seems like it needs an update?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:287 on 2025-02-20 22:56_

If we're renaming this anyway, would it make sense to rename it to `narrowing_constraints` to better distinguish it from visibility constraints?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:513 on 2025-02-20 23:05_

would `narrowing_constraints` also be a clearer name here, given that visibility constraints also apply to a binding?)

(and if we used that name consistently, the unpack would be simpler too)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:356 on 2025-02-20 23:12_

```suggestion
    /// You can add a `Constraint` multiple times, yielding different `ScopedConstraintId`s, which
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:360 on 2025-02-20 23:13_

Using `ScopedConstraintId` here means we have to be sure that a `VisibilityConstraints` is also scoped to a single... scope. This is the case, and there seems little chance it would ever not be the case... but it might be worth a doc comment mention anyway?

---

_@carljm approved on 2025-02-20 23:17_

This looks great, nice simplification.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:24 on 2025-02-21 13:45_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:287 on 2025-02-21 13:46_

This arena stores the `Constraint` objects that are used for both narrowing and visibility constraints, so I don't think the more precise name is accurate.  (And in another PR I'm working on, I would add a new arena to store data that is specific to narrowing constraints (and therefore called `narrowing_constraints`), and this would remain here to hold the shared data)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:513 on 2025-02-21 13:48_

For this one I agree with you! Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:360 on 2025-02-21 13:50_

There is one:

https://github.com/astral-sh/ruff/blob/7e6c1c2b4ae82316bf467f63fb6b93d056069246/crates/red_knot_python_semantic/src/visibility_constraints.rs#L284-L285

I can reword it if you feel it's not worded strongly enough

---

_@dcreager reviewed on 2025-02-21 13:50_

---

_Merged by @dcreager on 2025-02-21 14:16_

---

_Closed by @dcreager on 2025-02-21 14:16_

---

_Branch deleted on 2025-02-21 14:16_

---
