```yaml
number: 21957
title: "[ty] Allow gradual lower/upper bounds in a constraint set"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/gradual-bounds
created_at: 2025-12-12T20:05:24Z
updated_at: 2025-12-13T03:18:32Z
url: https://github.com/astral-sh/ruff/pull/21957
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Allow gradual lower/upper bounds in a constraint set

---

_Pull request opened by @dcreager on 2025-12-12 20:05_

We now allow the lower and upper bounds of a constraint to be gradual. Before, we would take the top/bottom materializations of the bounds. This required us to pass in whether the constraint was intended for a subtyping check or an assignability check, since that would control whether we took the "restrictive" or "permissive" materializations, respectively.

Unfortunately, doing so means that we lost information about whether the original query involves a non-fully-static type. This would cause us to create specializations like `T = object` for the constraint `T ≤ Any`, when it would be nicer to carry through the gradual type and produce `T = Any`.

We're not currently using constraint sets for subtyping checks, nor are we going to in the very near future. So for now, we're going to assume that constraint sets are always used for assignability checks, and allow the lower/upper bounds to not be fully static. Once we get to the point where we need to use constraint sets for subtyping checks, we will consider how best to record this information in constraints.

---

_Review requested from @carljm by @dcreager on 2025-12-12 20:05_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-12 20:05_

---

_Label `internal` added by @dcreager on 2025-12-12 20:05_

---

_Review requested from @sharkdp by @dcreager on 2025-12-12 20:05_

---

_Label `ty` added by @dcreager on 2025-12-12 20:05_

---

_Comment by @dcreager on 2025-12-12 20:06_

As written, note that this PR still takes the materializations of gradual typevar bounds and constraints. That deserves its own think, but is not as simple of a change.

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 20:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 20:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @carljm on 2025-12-12 20:53_

Reviewing now... I'm seeing one failing test in CI? Is this something non-deterministic?

I think materializing gradual upper bounds / constraints is likely fine for now (and maybe for always).

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:75 on 2025-12-12 20:55_

Nice, this seems like the right solution here

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:169 on 2025-12-12 20:56_

These are `None` because we have no basis for picking between `Base` and `Unrelated`, so we'll just fall back to `Unknown`?

---

_@carljm approved on 2025-12-12 20:57_

Looks good!

---

_Comment by @carljm on 2025-12-12 20:58_

> I think materializing gradual upper bounds / constraints is likely fine for now (and maybe for always).

Well, maybe not for always -- I just realized that not doing this may be part of the fix for all those TODOs in the tests around bound-by-gradual and constrained-by-gradual typevars, where we want to solve to the actual gradual constraint. But I think this is much lower priority.

---

_Comment by @carljm on 2025-12-13 00:03_

The failing test is an intersection ordering, and it passes for me locally, so it looks like a non-deterministic Salsa IDs issue.

Not sure if there's a cleverer solution inside the constraint-set implementation, but an easy (and I think cheap) solution would be to just normalize the ordering of those intersection types via `union_or_intersection_elements_ordering`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:169 on 2025-12-13 02:19_

That's right

---

_@dcreager reviewed on 2025-12-13 02:43_

> Not sure if there's a cleverer solution inside the constraint-set implementation, but an easy (and I think cheap) solution would be to just normalize the ordering of those intersection types via `union_or_intersection_elements_ordering`.

Done

---

_Merged by @dcreager on 2025-12-13 03:18_

---

_Closed by @dcreager on 2025-12-13 03:18_

---

_Branch deleted on 2025-12-13 03:18_

---
