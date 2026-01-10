```yaml
number: 1732
title: Fix spurious unions of generics from fixpoint iteration
type: issue
state: open
author: carljm
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-12-02T20:25:25Z
updated_at: 2025-12-12T08:20:17Z
url: https://github.com/astral-sh/ty/issues/1732
synced_at: 2026-01-10T01:56:40Z
```

# Fix spurious unions of generics from fixpoint iteration

---

_Issue opened by @carljm on 2025-12-02 20:25_

When we fixpoint-iterate a cycle, we union the resulting types at each iteration with the previous iteration, to ensure monotonicity.

This has an unfortunate side effect when the types involved are invariant generics or generics specialized with dynamic types; in those cases the unions don't simplify, and this can lead to undesired behavior. See for example the TODO `unsupported-base` errors in `mdtest/generics/legacy/classes.md`, or the TODOs for spurious unions with Divergent added in `call/methods.md` in https://github.com/astral-sh/ruff/pull/21551. These can both occur in reasonably common cases and show up in the ecosystem.

(The latter TODOs might also be resolvable by fixing https://github.com/astral-sh/ty/issues/1729 to eliminate the seemingly-unnecessary cycle.)

---

_Added to milestone `Beta` by @carljm on 2025-12-02 20:25_

---

_Label `bug` added by @carljm on 2025-12-02 20:25_

---

_Label `type-inference` added by @carljm on 2025-12-02 20:25_

---

_Comment by @mtshiba on 2025-12-04 03:40_

The problem is that when a query cycles, even if it doesn't actually diverge, `Divergent` or a calculation result derived from it will remain due to the union with the provisional value from the previous cycle.
However, the reason we must perform unioning is to suppress oscillations in type inference. If it's not oscillatory, it isn't necessary.
For example, suppose the type of the previous cycle of a type inference query is `list[Divergent]` and the type of the current cycle is `list[int]`. In this case, it seems unnecessary to union them into `list[Divergent] | list[int]`.
This is because if this type actually diverges recursively, the type of the current cycle would be something like `list[list[Divergent]]`.
The fact that this is not the case suggests that `Divergent` can actually be removed. For example, suppose the query uses the result of `infer_definition_types`. `Divergent` in the result of `infer_definition_types` will be removed in subsequent cycles, becoming something like `int`, but the problem is that the query depends on it will continue to retain the previous result of `infer_definition_type` due to the union.

Also, because this spurious `Divergent` has been removed, it seems unlikely that a type that was once determined to be `list[int]` can revert to `list[Divergent]` in a subsequent cycle.

---

_Assigned to @carljm by @MichaReiser on 2025-12-05 15:47_

---

_Comment by @carljm on 2025-12-11 01:11_

@mtshiba I played around with some ideas based on your suggestion above, and https://github.com/astral-sh/ruff/pull/21910 is the result so far. Still need to better understand the ecosystem impacts there.

---

_Comment by @carljm on 2025-12-12 03:54_

I think there is likely more to be done here, but with the two merged fixes I consider the current state good enough for beta. We'll see what comes up in user reports to prioritize further work.

---

_Removed from milestone `Beta` by @carljm on 2025-12-12 03:54_

---

_Added to milestone `Stable` by @carljm on 2025-12-12 03:54_

---

_Unassigned @carljm by @carljm on 2025-12-12 03:54_

---

_Comment by @sharkdp on 2025-12-12 08:20_

The ecosystem diff on https://github.com/astral-sh/ruff/pull/21935 demonstrates one case where we still produce unions with `Divergent`. We currently handle this with an explicit check (see diff in that PR), but eventually, we are going to want to remove the `asynccontexmanager` special handling, in which case this could pop up again.

---
