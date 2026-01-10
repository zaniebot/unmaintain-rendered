```yaml
number: 15539
title: "[red-knot] Implement disjointness for Instance types where the underlying class is `@final`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/final-instances
created_at: 2025-01-16T19:54:02Z
updated_at: 2025-01-16T23:50:54Z
url: https://github.com/astral-sh/ruff/pull/15539
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Implement disjointness for Instance types where the underlying class is `@final`

---

_Pull request opened by @AlexWaygood on 2025-01-16 19:54_

## Summary

Closes https://github.com/astral-sh/ruff/issues/15508

For any two instance types `T` and `S`, we know they are disjoint if either `T` is final and `T` is not a subclass of `S` or `S` is final and `S` is not a subclass of `T`.

Correspondingly, for any two types `type[T]` and `S` where `S` is an instance type, `type[T]` can be said to be disjoint from `S` if `S` is disjoint from `U`, where `U` is the type that represents all instances of `T`'s metaclass.

And a heterogeneous tuple type can be said to be disjoint from an instance type if the instance type is disjoint from `tuple` (a type representing all instances of the `tuple` class at runtime).

## Test Plan

- A new mdtest added. Most of our `is_disjoint_from()` tests are not written as mdtests just yet, but it's pretty hard to test some of these edge cases from a Rust unit test!
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`

Co-authored-by: Carl Meyer <carl@astral.sh>

---

_Label `red-knot` added by @AlexWaygood on 2025-01-16 19:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-16 19:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-16 19:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-16 19:54_

---

_@AlexWaygood reviewed on 2025-01-16 19:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1292 on 2025-01-16 19:55_

@carljm I realised almost as soon as our pairing session here that we were missing some subtle edge cases where a `type[T]` type could still be disjoint from an instance type even if the instance type was a subtype of `type`. So this logic is a little fiddlier than what we initially implemented. See the mdtest I've added for examples!

---

_Comment by @github-actions[bot] on 2025-01-16 20:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1292 on 2025-01-16 22:59_

Did a property test reveal this, or you just realized it looking at the code/tests?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:11 on 2025-01-16 23:01_

"associated class" doesn't seem like a well-defined term to me. Do we really just mean "the class of the instance type"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2025-01-16 23:04_

Nice! I like that we can delegate to instance-vs-instance, rather than checking finality here also.

It seems right that `T` actually disappears from this check, apart from its metaclass. There's no difference between `type[object]` and `type[OtherClass]` as regards disjointness from an instance type, as long as the metaclass of `OtherClass` is just `type`.

---

_@carljm approved on 2025-01-16 23:06_

Net reduction in lines of code (excluding tests), and improves correctness! Love to see it.

---

_@AlexWaygood reviewed on 2025-01-16 23:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1292 on 2025-01-16 23:13_

Just me reading the diff on github prior to filing the PR!

---

_@AlexWaygood reviewed on 2025-01-16 23:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:11 on 2025-01-16 23:21_

Both kinda feel like word salad to me, but I can go with "the class of the instance type" if you find it clearer 😆

---

_@carljm reviewed on 2025-01-16 23:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:11 on 2025-01-16 23:30_

I do :) 
```suggestion
class of the instance type is not a subclass of `T`'s metaclass.
```

---

_Merged by @AlexWaygood on 2025-01-16 23:48_

---

_Closed by @AlexWaygood on 2025-01-16 23:48_

---

_Branch deleted on 2025-01-16 23:48_

---
