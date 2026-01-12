```yaml
number: 15511
title: "[red-knot] Simplify `object` out of intersections"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/object-intersections
created_at: 2025-01-15T19:40:55Z
updated_at: 2025-01-15T20:08:06Z
url: https://github.com/astral-sh/ruff/pull/15511
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Simplify `object` out of intersections

---

_@AlexWaygood_

## Summary

`object & int` is equivalent to `int`, `object & Unknown` is equivalent to `Unknown`, `object & ~int` is equivalent to `~int`... I could go on ;)

`object` is the top type, the supertype of everything, so it is always redundant in an intersection. Eagerly simplifying it out of intersections is easy, and fixes some property-test failures in another branch of mine.

## Test Plan

- `cargo test -p red_knot_python_semantic --test mdtest
- `QUICKCHECK_TESTS=200000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`
- `uvx pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-15 19:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-15 19:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-15 19:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-15 19:40_

---

_Comment by @github-actions[bot] on 2025-01-15 19:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-01-15 19:49_

I recently wrote this down (in an unpublished branch):

> `object` describes the set of all possible values, while `Never` describes the empty set. The two types are complements of each other:
> 
> ```py
> from knot_extensions import static_assert, is_equivalent_to, Not
> from typing_extensions import Never
> 
> static_assert(is_equivalent_to(Not[object], Never))
> static_assert(is_equivalent_to(Not[Never], object))
> ```
> 
> This duality is also reflected in other facts:
> 
> - `Never` is a subtype of every type, while `object` is a supertype of every type.
> - `Never` is assignable to every type, while `object` is assignable from every type.
> - `Never` is disjoint from every type, while `object` overlaps with every type.
> - Building a union with `Never` is a no-op, intersecting with `object` is a no-op.
> - Interecting with `Never` results in `Never`, building a union with `object` results in `object`.

Thanks for explicitly implementing the "intersecting with `object` is a no-op" part of this :+1: 

---

_@sharkdp reviewed on 2025-01-15 19:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:43 on 2025-01-15 19:52_

Maybe this could go in `intersection_types.md` instead? Maybe below the "`Never` in intersections" section, as a new "`object` in intersections" section?

---

_@AlexWaygood reviewed on 2025-01-15 19:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:43 on 2025-01-15 19:53_

Ah yes, that's a better place for this, thanks!

---

_@sharkdp reviewed on 2025-01-15 19:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:296 on 2025-01-15 19:56_

I think I would prefer to split that into two parts for readability(?)
```suggestion
                if known_instance == Some(KnownClass::Object) {
                    // `object & T` -> `T`
                    return;
                }

                let addition_is_bool_instance = known_instance == Some(KnownClass::Bool);
```

---

_@sharkdp approved on 2025-01-15 19:57_

Thank you!

---

_@AlexWaygood reviewed on 2025-01-15 19:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:296 on 2025-01-15 19:57_

I wondered about that... I assumed the single `match` would be better for performance, but maybe I'm prematurely optimising (or underestimating the compiler's ability to optimise this code)

---

_@carljm approved on 2025-01-15 20:01_

---

_Merged by @AlexWaygood on 2025-01-15 20:06_

---

_Closed by @AlexWaygood on 2025-01-15 20:06_

---

_Branch deleted on 2025-01-15 20:06_

---
