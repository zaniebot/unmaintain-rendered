```yaml
number: 16314
title: "[red-knot] Support `TypeGuard` and `TypeIs`"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - ty
assignees: []
draft: true
base: main
head: rk-type-guards
created_at: 2025-02-22T12:17:43Z
updated_at: 2025-07-09T20:32:05Z
url: https://github.com/astral-sh/ruff/pull/16314
synced_at: 2026-01-10T18:33:12Z
```

# [red-knot] Support `TypeGuard` and `TypeIs`

---

_Pull request opened by @InSyncWithFoo on 2025-02-22 12:17_

## Summary

Part of #13694.

This PR's description is currently empty. See [this comment](https://github.com/astral-sh/ruff/pull/16314#issuecomment-2676178735) for a list of unresolved major issues.

## Test Plan

Markdown tests.


---

_Label `red-knot` added by @AlexWaygood on 2025-02-22 12:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4377 on 2025-02-22 12:30_

It seems that the two types are exactly identical except for the name. Could we use a shared type (similar to `Class`) or make this type only thin wrapper types around a shared type? It would avoid the need for a macro

---

_@MichaReiser reviewed on 2025-02-22 12:30_

---

_Comment by @InSyncWithFoo on 2025-02-22 12:30_

This PR is incomplete (hence the empty description). Some major issues (all reflected in tests):

* Invalid definitions are supposed to be reported.
	I thought the logic could be added to `.infer_function_definition()`, but calling `.infer_type_expression()`/`.infer_type_expression_no_store()` there (on `returns`) leads to panic.

* `if b` where `b` is a bound guard is not recognized correctly.
	`.evaluate_expr_name()` needs some changes. However, I'm not sure if this approach scales, since `b` might not be a name.

* `TypeGuard[]`-based constraints should overrule all other constraints
	I have yet to find out a way to do this. It seems that constraints are considered always invertible and compatible with one another.

---

_@MichaReiser reviewed on 2025-02-22 12:32_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/narrow/type_guards.md`:44 on 2025-02-22 12:32_

I'm not sure if it's possible or reasonable here but I found many very specific mdtests much more helpful when debugging regression than a few tests that have many unrelated assertions. 

There are also fewer trade-offs when using smaller tests compared to Ruff because it doesn't require you to create a new file. A single header is enough to create an isolated test.

---

_@InSyncWithFoo reviewed on 2025-02-22 12:33_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:4377 on 2025-02-22 12:33_

I started with that, but the various `match`/`case` function ended up quite unreadable, not to mention the subtle differences (e.g., `TypeGuard[]` is covariant, but `TypeIs[]` is invariant). All in all, I think the current approach is the best, despite being a little bit non-DRY.

---

_@InSyncWithFoo reviewed on 2025-02-22 12:38_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/narrow/type_guards.md`:44 on 2025-02-22 12:38_

Aren't all of these about function arity? I think this test is pretty small already, unless you mean the second file below (which is for subtype relation in `TypeIs` functions)?

---

_Comment by @MichaReiser on 2025-05-18 16:34_

I think this PR has become stale or do you plan on continue working on this PR?

---

_Comment by @InSyncWithFoo on 2025-05-18 17:03_

@MichaReiser I'm awaiting @carljm's reply at [#117](https://github.com/astral-sh/ty/issues/117). In the meanwhile, I think I can at least split the working `TypeIs` parts into a separate PR; I'll do that next week.

---

_Comment by @MichaReiser on 2025-05-18 18:24_

Thanks for the update :) I just went through some older PRs and it wasn't clear to me what we're waiting on for this one.

---

_Comment by @carljm on 2025-05-22 03:40_

@InSyncWithFoo sorry I lost track of that! Replied now, let me know if you have other questions. Thanks for your work on this, and sorry we let it get stale!

---

_Comment by @InSyncWithFoo on 2025-05-25 01:18_

Opened #18294 as a replacement for this one, since rebasing this would take too much effort.

---

_Closed by @InSyncWithFoo on 2025-05-25 01:18_

---

_Branch deleted on 2025-07-09 20:32_

---
