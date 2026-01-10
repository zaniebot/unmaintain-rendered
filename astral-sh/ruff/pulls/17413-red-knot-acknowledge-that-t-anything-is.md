```yaml
number: 17413
title: "[red-knot] Acknowledge that `T & anything` is assignable to `T`"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/typevar-minus
created_at: 2025-04-15T19:27:20Z
updated_at: 2025-04-15T20:34:09Z
url: https://github.com/astral-sh/ruff/pull/17413
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Acknowledge that `T & anything` is assignable to `T`

---

_Pull request opened by @dcreager on 2025-04-15 19:27_

This reworks the assignability/subtyping relations a bit to handle typevars better:

1. For the most part, types are not assignable to typevars, since there's no guarantee what type the typevar will be specialized to.

2. An intersection is an exception, if it contains the typevar itself as one of the positive elements. This should fall out from the other clauses automatically, since a typevar is assignable to itself, and an intersection is assignable to something if any positive element is assignable to that something.

3. Constrained typevars are an exception, since they must be specialized to _exactly_ one of the constraints, not to a _subtype_ of a constraint. If a type is assignable to every constraint, then the type is also assignable to the constrained typevar.

We already had a special case for (3), but the ordering of it relative to the intersection clauses meant we weren't catching (2) correctly.  To fix this, we keep the special case for (3), but fall through to the other match arms for non-constrained typevars and if the special case isn't true for a constrained typevar.

Closes https://github.com/astral-sh/ruff/issues/17364

---

_Label `red-knot` added by @dcreager on 2025-04-15 19:27_

---

_Review requested from @carljm by @dcreager on 2025-04-15 19:27_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-15 19:27_

---

_Review requested from @sharkdp by @dcreager on 2025-04-15 19:27_

---

_Comment by @github-actions[bot] on 2025-04-15 19:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:895 on 2025-04-15 19:31_

This comment is true, but I have a hard time understanding how it's relevant to the code below. Would we write this arm any differently if constraints had to be disjoint? It would still be the case that the lhs type must be a subtype of _all_ constraints (separately; not just the union of them) in order to be a subtype of the constrained typevar, which seems like the more important point to make here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1193 on 2025-04-15 19:32_

Same question as above.

---

_@carljm approved on 2025-04-15 19:32_

Excellent, thank you!

---

_@dcreager reviewed on 2025-04-15 19:57_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:895 on 2025-04-15 19:57_

My thinking was that the possibility of non-disjoint constraints is the only reason that this arm is needed. If the constraints are disjoint, I don't think there's any lhs that could be a subtype of all of them, is there?

---

_@carljm reviewed on 2025-04-15 20:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:895 on 2025-04-15 20:28_

Oh! Yes, that totally makes sense :) Disregard this comment.

---

_Merged by @dcreager on 2025-04-15 20:34_

---

_Closed by @dcreager on 2025-04-15 20:34_

---

_Branch deleted on 2025-04-15 20:34_

---
