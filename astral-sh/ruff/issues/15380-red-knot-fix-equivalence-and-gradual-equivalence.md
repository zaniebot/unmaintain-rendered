```yaml
number: 15380
title: "[red-knot] fix equivalence (and gradual equivalence) for differently-ordered unions and intersections"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-09T18:15:15Z
updated_at: 2025-01-19T16:10:44Z
url: https://github.com/astral-sh/ruff/issues/15380
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] fix equivalence (and gradual equivalence) for differently-ordered unions and intersections

---

_@carljm_

This issue relates to the TODO comment here:

https://github.com/astral-sh/ruff/blob/b0905c4b043189879af78afb757270c4e0397e25/crates/red_knot_python_semantic/src/types.rs#L1065-L1081

Some ~raw comments from discussion about this issue:

---

it's not simple, but it also shouldn't be worse than the existing `is_subtype_of` support for intersections and unions, I think; it just feels wrong that it's `O(n^2)`
i mean the simplest (but maybe not most efficient) implementation of it would be "is `A` a subtype of `B` and `B` a subtype of `A`"
if so, they are equivalent

**Alex Waygood — Today at 10:01 AM**

except that `is_subtype_of()` calls `is_equivalent_to()`  😆
so you'll get infinite recursion with our current structure

**carljm — Today at 10:01 AM**

true, we may need to split atomic equivalence
because we do maintain a simplified two-layer structure for unions and intersections
we don't have arbitrary recursive structures
so we should know when handling union/intersection subtyping when we won't be dealing with more unions/intersections 

**Alex Waygood — Today at 10:02 AM**

I do also think that we can implement optimisations by making sure that equivalent types have the same internal representation wherever possible, like the [thing I implemented the other day](https://github.com/astral-sh/ruff/pull/15272) so that `type[object]` is eagerly simplified to `type`

---

_Label `red-knot` added by @carljm on 2025-01-09 18:15_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 18:15_

---

_Comment by @carljm on 2025-01-09 19:38_

In our 1:1 just now @sharkdp raised the possibility that we could do a more efficient version of this based on consistently ordering unions and intersections according to "equivalence classes" of types (so that for example `Any` and `Unknown` would always be ordered adjacent to each other).

This would require that we track "user-facing" order (i.e. to preserve the user-provided order) separately, or just decide that we're doing enough type simplification that tracking this user-facing order actually doesn't matter.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-13 15:17_

---

_Closed by @AlexWaygood on 2025-01-19 16:10_

---
