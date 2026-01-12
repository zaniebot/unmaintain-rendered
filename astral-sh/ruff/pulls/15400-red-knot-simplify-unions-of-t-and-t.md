```yaml
number: 15400
title: "[red-knot] Simplify unions of T and ~T"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/union-of-type-and-its-complement
created_at: 2025-01-10T13:43:17Z
updated_at: 2025-05-07T15:22:47Z
url: https://github.com/astral-sh/ruff/pull/15400
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Simplify unions of T and ~T

---

_@sharkdp_

## Summary

Simplify unions of `T` and `~T` to `object`.

## Test Plan

Adapted existing tests.

---

_Label `red-knot` added by @sharkdp on 2025-01-10 13:43_

---

_Review requested from @carljm by @sharkdp on 2025-01-10 13:43_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-10 13:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-10 13:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:24 on 2025-01-10 13:43_

In these first four, only the order changed slightly.

---

_@sharkdp reviewed on 2025-01-10 13:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:90 on 2025-01-10 13:47_

According to the descriptions in https://github.com/astral-sh/ruff/pull/14687, we have

```
~AlwaysTruthy = Falsy
~AlwaysFalsy = Truthy
```

and

```
Truthy | Falsy = U = object (U is the Universal Set in set theory)
```

So we can rewrite what we had before to prove that this is correct:

```
  A & ~AlwaysTruthy | B & ~AlwaysTruthy | A & ~AlwaysFalsy | B & ~AlwaysFalsy
= A & Falsy | B & Falsy | A & Truthy | B & Truthy
= A & Falsy | B & Falsy | A & Truthy | B & Truthy
= A & Falsy | A & Truthy | B & Falsy | B & Truthy
= A & (Falsy | Truthy) | B & (Falsy | Truthy)
= A & object | B & object
= A | B
```

---

_@sharkdp reviewed on 2025-01-10 13:47_

---

_@sharkdp reviewed on 2025-01-10 13:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:93 on 2025-01-10 13:48_

Same proof as above, just starts with a reordered union.

---

_@sharkdp reviewed on 2025-01-10 13:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:217 on 2025-01-10 13:49_

Similar to above:
```
  A & ~AlwaysTruthy | A & ~AlwaysFalsy
= A & Falsy | A & Truthy
= A & (Falsy | Truthy)
= A & object
= A
```

---

_Comment by @github-actions[bot] on 2025-01-10 13:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-01-10 13:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:219 on 2025-01-10 13:51_

This PR does not resolve #15023, but we can remove the TODO comment nevertheless.

---

_@sharkdp reviewed on 2025-01-10 13:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:90 on 2025-01-10 13:56_

To show that *no* narrowing happens (i.e. that we still have `A | B`) also seems to be the intention of this test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:85 on 2025-01-10 16:39_

Is there an argument for inlining the negation to ensure we avoid doing it if one of the above cases matches? We only use `ty_negated` once.

```suggestion
                    } else if ty.negate(self.db).is_subtype_of(self.db, *element) {
```

---

_@carljm approved on 2025-01-10 16:39_

Nice!

---

_@sharkdp reviewed on 2025-01-10 21:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:85 on 2025-01-10 21:48_

> We only use `ty_negated` once.

It's still a loop though, so we would compute it for every pre-existing element of the union, no?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:85 on 2025-01-10 21:56_

Oh, I totally missed that, you're right. Let's leave it as is. If this shows up as a significant cost, I think there would be ways we can make it more efficient. For example, we could have a dedicated `negation_is_subtype_of` that would short circuit the most costly cases (where `ty` is a simple type and `ty_negated` requires constructing an intersection, in which case we will never be able to declare the resulting negation type a subtype of anything other than `object` or another negation type) and defer to `self.negate(db).is_subtype_of(...)` for the remainder.

---

_@carljm approved on 2025-01-10 21:56_

---

_Merged by @sharkdp on 2025-01-10 22:00_

---

_Closed by @sharkdp on 2025-01-10 22:00_

---

_Branch deleted on 2025-01-10 22:00_

---
