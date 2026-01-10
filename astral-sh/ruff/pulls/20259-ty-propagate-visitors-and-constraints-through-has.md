```yaml
number: 20259
title: "[ty] propagate visitors and constraints through has_relation_in_invariant_position"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/recursive-invariant-relation
created_at: 2025-09-05T02:06:59Z
updated_at: 2025-09-06T00:18:25Z
url: https://github.com/astral-sh/ruff/pull/20259
synced_at: 2026-01-10T17:46:21Z
```

# [ty] propagate visitors and constraints through has_relation_in_invariant_position

---

_Pull request opened by @carljm on 2025-09-05 02:06_

## Summary

The sub-checks for assignability and subtyping of materializations performed in `has_relation_in_invariant_position` and `is_subtype_in_invariant_position` need to propagate the `HasRelationToVisitor`, or we can stack overflow.

A side effect of this change is that we also propagate the `ConstraintSet` through, rather than using `C::from_bool`, which I think may also become important for correctness in cases involving type variables (though it isn't testable yet, since we aren't yet actually creating constraints other than always-true and always-false.)

## Test Plan

Added mdtest (derived from code found in pydantic) which stack-overflowed before this PR.

With this change incorporated, pydantic now checks successfully on my draft PR for PEP 613 TypeAlias support.


---

_Label `ty` added by @carljm on 2025-09-05 02:07_

---

_Comment by @github-actions[bot] on 2025-09-05 02:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-05 02:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-05 02:20_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-05 02:20_

---

_Review requested from @sharkdp by @carljm on 2025-09-05 02:20_

---

_Review requested from @dcreager by @carljm on 2025-09-05 02:20_

---

_Comment by @carljm on 2025-09-05 02:20_

@JelleZijlstra I can't request your review here, as a non-member of the GH org, but I wouldn't mind your eyes on this if you have a chance, since this is updating code you wrote.

---

_@JelleZijlstra reviewed on 2025-09-05 03:34_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:300 on 2025-09-05 03:34_

This test case is a bit weird, because the `JsonSchemaExtraCallable` alias only contains callables. If this test case fails without your fix, it's fine, but wonder if something important got lost.

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 03:36_

This becomes harder and harder to read, which is unfortunate as the logic is a bit tricky. But oh well.

---

_@JelleZijlstra reviewed on 2025-09-05 03:37_

Looks good! (I'd approve but I'm not allowed to do that either :) .)

---

_@carljm reviewed on 2025-09-05 05:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:300 on 2025-09-05 05:08_

Yes -- this doesn't simplify because `dict` and `Callable` are not disjoint, so it remains an intersection of `Top[dict[Unknown, Unknown]] & JsonSchemaExtraCallable` (which unpacks into a union of two intersections, since `JsonSchemaExtraCallable` is a union of two callable types -- the issue doesn't repro if I simplify it to a single callable type). Calling the overloaded `dict.update` (calling overloads can trigger materialization) with that union-of-intersections type is necessary to trigger the issue.

Clearly a lot of the original relevant code "got lost" in minimization. What's left is both sufficient and seemingly necessary to trigger the issue, but doesn't make a lot of sense as a standalone snippet.

I will spend some more time tomorrow understanding the precise mechanism more fully so I can write some more thorough and low-level tests.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 05:09_

I just pushed an update that I think helps a lot by defining an `is_subtype_of` closure helper, to remove all the repeated `TypeRelation::Subtyping, visitor` noise.

---

_@carljm reviewed on 2025-09-05 05:09_

---

_Comment by @carljm on 2025-09-05 05:22_

@dcreager One thing to note here is that with invariance we sometimes end up checking type equivalence or subtyping as a sub-part of checking outer assignability, and with this PR we are taking constraints generated in an inner subtype or equivalence check and propagating them up to an outer assignability check. I think this is OK because the constraints we collect always use top and bottom materializations and fully static types, so the constraints should always be valid/interchangeable across assignability and subtyping? And I assume for equivalence we will create constraints with the same upper and lower bound, so those constraints are also valid across type relations. But let me know if there are issues here I'm not seeing.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:490 on 2025-09-05 09:14_

could this also be written as

```suggestion
            .or(db, || {
                is_subtype_of(base_bottom, derived_top)
                    .and(db, || is_subtype_of(derived_top, base_top))
            })
            .or(db, || {
                is_subtype_of(base_top, derived_top)
                    .and(db, || is_subtype_of(derived_bottom, base_top))
            })
```

?

---

_@AlexWaygood approved on 2025-09-05 09:15_

---

_Comment by @dcreager on 2025-09-05 12:26_

> One thing to note here is that with invariance we sometimes end up checking type equivalence or subtyping as a sub-part of checking outer assignability, and with this PR we are taking constraints generated in an inner subtype or equivalence check and propagating them up to an outer assignability check. I think this is OK because the constraints we collect always use top and bottom materializations and fully static types, so the constraints should always be valid/interchangeable across assignability and subtyping? And I assume for equivalence we will create constraints with the same upper and lower bound, so those constraints are also valid across type relations. But let me know if there are issues here I'm not seeing.

I think that should be okay, because the conditions under which that inner equivalence holds _should_ also be part of the conditions under which the outer assignability holds. (And you're right that we shouldn't end up with incompatible constraints or execution cycles since all of the checks that happen down inside the constraint set machinery work on fully static / materialized types.)

If we find that :point_up: isn't correct, then you can call `C::from_bool(is_equivalent_to)` instead of `when_equivalent_to`. (Though at that point we might consider adding `ensure_{always,sometimes}_satisfied` helpers on the `Constraints` trait.)

---

_@dcreager reviewed on 2025-09-05 12:28_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 12:28_

> This becomes harder and harder to read

I had tried to use operator overloading when I first wrote these, but unfortunately you can't overload the short-circuiting `&&` and `||` operators. These closures are the only way I know of to get the rhs into a thunk so that it's lazily evaluated.

(Hmm...maybe a macro that would rewrite the boolean syntax into the right `and` and `or` calls? An interesting idea but not something to focus on atm)

---

_@carljm reviewed on 2025-09-05 12:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 12:55_

Short of a macro or operator overloading, I did consider whether in nested cases like this a prefix-form helper would improve readability. So it would change `a.or(db, || b)` to `or(db, a, || b)`. 

I do think the prefix form is somewhat better visually for nested conditions. It doesn't allow us to get rid of the `||` since we still want short circuiting. I decided that limitation made it not worth it. 

---

_@dcreager reviewed on 2025-09-05 14:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 14:22_

Oh that's a good idea!  I think that will work without changing the trait or adding extra methods if you spell it as `C::or(a, db, || b)`.  (The weird parameter order is admittedly not great)

---

_@JelleZijlstra reviewed on 2025-09-05 15:18_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 15:18_

Thanks, this already looks much better with the private helper.

---

_@carljm reviewed on 2025-09-05 21:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:484 on 2025-09-05 21:44_

Yeah I think what I've got is good enough here for now, I probably won't pursue `C::or` as part of this PR; the fact that we still can't get rid of the `db` or the `||` makes it feel less appealing.

---

_Merged by @carljm on 2025-09-06 00:17_

---

_Closed by @carljm on 2025-09-06 00:17_

---

_Branch deleted on 2025-09-06 00:17_

---
