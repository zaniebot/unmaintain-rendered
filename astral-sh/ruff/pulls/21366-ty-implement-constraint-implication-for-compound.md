```yaml
number: 21366
title: "[ty] Implement constraint implication for compound types"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/deep-comparison
created_at: 2025-11-10T17:20:58Z
updated_at: 2025-11-14T23:43:02Z
url: https://github.com/astral-sh/ruff/pull/21366
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Implement constraint implication for compound types

---

_Pull request opened by @dcreager on 2025-11-10 17:20_

This PR updates the constraint implication type relationship to work on compound types as well. (A compound type is a non-atomic type, like `list[T]`.)

The goal of constraint implication is to check whether the requirements of a constraint imply that a particular subtyping relationship holds. Before, we were only checking atomic typevars. That would let us verify that the constraint set `T ≤ bool` implies that `T` is always a subtype of `int`. (In this case, the lhs of the subtyping check, `T`, is an atomic typevar.)

But we weren't recursing into compound types, to look for nested occurrences of typevars. That means that we weren't able to see that `T ≤ bool` implies that `Covariant[T]` is always a subtype of `Covariant[int]`.

Doing this recursion means that we have to carry the constraint set along with us as we recurse into types as part of `has_relation_to`, by adding constraint implication as a new `TypeRelation` variant. (Before it was just a method on `ConstraintSet`.)

---

_Label `internal` added by @dcreager on 2025-11-10 17:20_

---

_Review requested from @carljm by @dcreager on 2025-11-10 17:20_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-10 17:21_

---

_Review requested from @sharkdp by @dcreager on 2025-11-10 17:21_

---

_Label `ty` added by @dcreager on 2025-11-10 17:21_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 17:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-10 17:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-10 21:20_

Are the primer hits here expected?

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-11 00:33_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 00:39_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://dcreager-deep-comparison.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-deep-comparison.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-11-11 01:08_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-11 01:08_

---

_Comment by @dcreager on 2025-11-11 01:08_

> Are the primer hits here expected?

They were not! Latest patch disappears them

---

_Assigned to @sharkdp by @sharkdp on 2025-11-11 08:17_

---

_Comment by @sharkdp on 2025-11-11 14:26_

I am planning to review this, but postponing to tomorrow, since Doug is out today anyway.

---

_Review request for @carljm removed by @carljm on 2025-11-11 19:55_

---

_Comment by @dcreager on 2025-11-11 20:26_

> I am planning to review this, but postponing to tomorrow, since Doug is out today anyway.

Thank you! No rush

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:10356 on 2025-11-12 16:21_

```suggestion
    /// whether `T ≤ int`, then the answer will depend on what constraint set you are considering:
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:10379 on 2025-11-12 16:24_

Is "constraint implication" the name in the literature? I think I would prefer something like
```suggestion
    SubtypingGivenPremises(ConstraintSet<'db>),
```
or
```suggestion
    SubtypingUnderAssumptions(ConstraintSet<'db>),
```
or
```suggestion
    SubtypingAssuming(ConstraintSet<'db>),
```
but I'm obviously also fine with what you have.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:224 on 2025-11-13 11:34_

Would it be useful to add tests with the typevar on the right hand side? For example
```suggestion
    static_assert(not given_bool.implies_subtype_of(Covariant[T], Covariant[str]))

    given_bool_le_t_le_int = ConstraintSet.range(bool, T, int)
    static_assert(not given_bool_le_t_le_int.implies_subtype_of(Covariant[int], Covariant[T]))
    static_assert(given_bool_le_t_le_int.implies_subtype_of(Covariant[bool], Covariant[T]))
    static_assert(not given_bool_le_t_le_int.implies_subtype_of(Covariant[str], Covariant[T]))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:348 on 2025-11-13 11:37_

Maybe test the reverse direction too?
```suggestion
    static_assert(given_int.implies_subtype_of(Invariant[T], Invariant[int]))
    static_assert(given_int.implies_subtype_of(Invariant[int], Invariant[T]))
    static_assert(not given_int.implies_subtype_of(Invariant[T], Invariant[bool]))
    static_assert(not given_int.implies_subtype_of(Invariant[bool], Invariant[T]))
    static_assert(not given_int.implies_subtype_of(Invariant[T], Invariant[str]))
    static_assert(not given_int.implies_subtype_of(Invariant[str], Invariant[T]))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1629 on 2025-11-13 11:39_

Maybe
```suggestion
        premises: ConstraintSet<'db>,
```
or
```suggestion
        assumptions: ConstraintSet<'db>,
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1706 on 2025-11-13 11:42_

I think I missed a few of your PRs, so I'm not sure if this came up before, but the naming here is a bit confusing to me. I think I would prefer something like below, but certainly no need to change anything here if this has been discussed before and/or if you prefer the current naming
```suggestion
            return premises.imply_subtype_of(db, self, target);
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:1460 on 2025-11-13 11:57_

This sounds like an optimization, but seems to be required for correctness?

---

_@sharkdp approved on 2025-11-13 12:01_

Thank you!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1460 on 2025-11-14 23:14_

Yep, as described (and clarified/fixed) in much more detail in https://github.com/astral-sh/ruff/pull/21463

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1706 on 2025-11-14 23:16_

Yes that also lines up more nicely with the name of the mdtest function. (That would change the tense slightly to `implies_subtype_of` which _technically_ doesn't agree with a plural `premises`, but I am going to choose not to worry about that.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:10379 on 2025-11-14 23:16_

> Is "constraint implication" the name in the literature?

Nope! I do like `SubtypingAssuming`, that also calls out the relationship to `TypeRelation::Subtyping` more clearly.

---

_@dcreager reviewed on 2025-11-14 23:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1629 on 2025-11-14 23:22_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1706 on 2025-11-14 23:22_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:10379 on 2025-11-14 23:22_

Done

---

_@dcreager reviewed on 2025-11-14 23:23_

---

_Merged by @dcreager on 2025-11-14 23:43_

---

_Closed by @dcreager on 2025-11-14 23:43_

---

_Branch deleted on 2025-11-14 23:43_

---
