```yaml
number: 17094
title: "[red-knot] Disjointness for callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-disjoint
created_at: 2025-03-31T14:04:11Z
updated_at: 2025-04-02T02:35:18Z
url: https://github.com/astral-sh/ruff/pull/17094
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Disjointness for callable types

---

_Pull request opened by @dhruvmanila on 2025-03-31 14:04_

## Summary

Part of #15382, this PR adds support for disjointness between two callable types. They are never disjoint because there exists a callable type that's a subtype of all other callable types:
```py
(*args: object, **kwargs: object) -> Never
```

The `Never` is a subtype of every fully static type thus a callable type that has the return type of `Never` means that it is a subtype of every return type.

## Test Plan

Add test cases related to mixed parameter kinds, gradual form (`...`) and `Never` type.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-31 14:04_

---

_Comment by @github-actions[bot] on 2025-03-31 14:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dhruvmanila reviewed on 2025-03-31 16:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:92 on 2025-03-31 16:36_

This is not related to this PR but it helped my understanding when I discussed `Never` usage with Alex and David last week so I added some test cases for it as they were missing.

---

_Marked ready for review by @dhruvmanila on 2025-04-01 14:10_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-01 14:10_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-01 14:10_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-01 14:10_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-01 14:10_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 14:42_

Maybe expand on _why_ it's similar to `Never`:

```suggestion
If a tuple type contains a `Never` element, then it cannot be instantiated. Like `Never`, the
type contains no objects, and is therefore disjoint from any other type.
```

---

_@dcreager reviewed on 2025-04-01 14:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 17:22_

I think this comment is slightly confusing. `tuple[int, Never]` is not _like_ `Never` in our model -- it literally _is_ `Never`! We view `tuple[int, Never]` and `Never` as being exactly the same type: `tuple[int, Never]` is just eagerly simplified to `Never` in our model; `Never` should never appear as a `Type::Tuple()` element in our representation https://playknot.ruff.rs/bd45448d-5c21-49db-ab98-093d57b24768

---

_@AlexWaygood reviewed on 2025-04-01 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 17:29_

It's great to add some tests for this. But I think I'd prefer just some tests like this; I think they'd be clearer:

```py
def f(x: tuple[Never], y: tuple[int, Never], z: tuple[Never, int]):
    reveal_type(x)  # revealed: Never
    reveal_type(y)  # revealed: Never
    reveal_type(z)  # revealed: Never
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:360 on 2025-04-01 17:30_

(optional)

```suggestion
`(*args: object, **kwargs: object) -> Never` that is a subtype of all fully static callable types.
As such, for any two callable types, it is possible to conceive of a runtime callable object that would
inhabit both types simultaneously.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1337 on 2025-04-01 17:35_

What's the reason for the asymmetry here? Could this be

```suggestion
            (
                Type::Callable(CallableType::General(_)) | Type::FunctionLiteral(_),
                Type::Callable(CallableType::General(_)) | Type::FunctionLiteral(_),
            ) => {
```

or

```suggestion
            (
                Type::Callable(CallableType::General(_)) | Type::FunctionLiteral(_),
                Type::Callable(CallableType::General(_)),
            )
            | (
                Type::Callable(CallableType::General(_)),
                Type::Callable(CallableType::General(_)) | Type::FunctionLiteral(_),
            ) => {
```

---

_@AlexWaygood approved on 2025-04-01 17:35_

Nice, this looks great. A couple of small points

---

_@dcreager reviewed on 2025-04-01 17:48_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 17:48_

> I think this comment is slightly confusing. `tuple[int, Never]` is not _like_ `Never` in our model -- it literally _is_ `Never`!

Ah, got it! Does that suggest that we want this to be an explicit invariant of our representation? Is our intent to have `Type::Never` be the _only_ empty type? Are there other types that we would need to make sure simplify to `Never`? (I know intersection with `Never` is already simplified like this.)

---

_@AlexWaygood reviewed on 2025-04-01 17:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 17:52_

It would certainly be nice if we _can_ maintain this invariant going forward! It makes a lot of things simpler to think about 😄 I can't say for sure whether it will always be feasible to eagerly detect (and simplify to `Never`) all types that are known to be empty, though

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 18:16_

Yes, I think we want this to be an invariant of our representation -- and I think it should be possible.

And I also agree that it would be better to test this simplification not in this file, and not using `is_disjoint_from`, but rather the way Alex suggests, and over in the tuple type tests.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:360 on 2025-04-01 18:19_

Similar to Alex's suggestion, but I think it's important to clarify that it's a non-empty subtype, because even disjoint types have the common subtype `Never`. And personally I think once we clarify that, it's sufficient without adding an entirely new sentence: if two types have a non-empty common subtype, then obviously they share some inhabitants. But I don't feel strongly, we can add another sentence driving that point home.

```suggestion
No two callable types are disjoint because there exists a non-empty callable type
`(*args: object, **kwargs: object) -> Never` that is a subtype of all fully static callable types.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1337 on 2025-04-01 18:20_

The second suggestion could work, but not the first. Two `FunctionLiteral` types can definitely be disjoint from each other (in fact they always are.)

I think the reason for the asymmetry in the current version is simply avoiding redundancy. The case of two general-callable-types is always handled by the first clause, so doesn't need to be represented in the second clause.

I don't think it matters much but IMO the clearest version might be to have three separate ORed outer clauses, with no interior OR: function-literal vs general-callable, general-callable vs function-literal, and general-callable vs general-callable.

---

_@carljm approved on 2025-04-01 18:24_

---

_@dhruvmanila reviewed on 2025-04-01 18:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1337 on 2025-04-01 18:28_

> The second suggestion could work, but not the first. Two `FunctionLiteral` types can definitely be disjoint from each other (in fact they always are.)

Yes, and they're handled a bit above in the match expression.

> I think the reason for the asymmetry in the current version is simply avoiding redundancy. The case of two general-callable-types is always handled by the first clause, so doesn't need to be represented in the second clause.

Yup, this is the reason (I was about to comment this :)).

> I don't think it matters much but IMO the clearest version might be to have three separate clauses: function-literal vs general-callable, general-callable vs function-literal, and general-callable vs general-callable.

I started out with this but clippy complains about "unnested or-patterns" which then suggests to collapse it into what the current pattern is in the PR. We would need to have separate branches for it to work but I think I'd want to avoid that so that the comment explanation remains present in the same branch for all related cases.

---

_@dhruvmanila reviewed on 2025-04-01 18:29_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:84 on 2025-04-01 18:29_

> Yes, I think we want this to be an invariant of our representation -- and I think it should be possible.

I can try and create a follow-up for this.

> And I also agree that it would be better to test this simplification not in this file, and not using `is_disjoint_from`, but rather the way Alex suggests, and over in the tuple type tests.

Make sense

---

_Merged by @dhruvmanila on 2025-04-01 19:00_

---

_Closed by @dhruvmanila on 2025-04-01 19:00_

---

_Branch deleted on 2025-04-01 19:00_

---

_@sharkdp reviewed on 2025-04-01 21:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/tuple.md`:15 on 2025-04-01 21:10_

We have existing tests for this in `type_properties/tuples_containing_never.md`. Can we merge these new tests into the existing test suite, in case they increase coverage?

---

_@dhruvmanila reviewed on 2025-04-02 02:35_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/tuple.md`:15 on 2025-04-02 02:35_

https://github.com/astral-sh/ruff/pull/17137

---
