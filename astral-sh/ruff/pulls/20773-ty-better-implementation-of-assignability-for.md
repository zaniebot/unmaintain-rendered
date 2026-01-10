```yaml
number: 20773
title: "[ty] Better implementation of assignability for intersections with negated gradual elements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/gradual-intersections
created_at: 2025-10-08T19:03:49Z
updated_at: 2025-10-10T19:19:02Z
url: https://github.com/astral-sh/ruff/pull/20773
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Better implementation of assignability for intersections with negated gradual elements

---

_Pull request opened by @AlexWaygood on 2025-10-08 19:03_

Our implementation of assignability between a type `T` and another type `~U` is currently too strict. We currently only consider `T` assignable to `~U` if `Top[T]` is disjoint from `Top[U]`. That's correct for subtyping and redundancy, but not for assignability: for assignability, we should be more permissive, and allow `T` to be considered assignable to `~U` if `Bottom[T]` is disjoint from `Bottom[U]`.

As part of this PR, I also improved the docstring of `Type::is_disjoint_from` to make it clear what it actually does (it tests for disjointness between the top materializations of the pair of types).

Fixes https://github.com/astral-sh/ty/issues/767

## Test plan

- Added mdtests that fail on `main`
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` locally

---

_Label `ty` added by @AlexWaygood on 2025-10-08 19:03_

---

_Comment by @github-actions[bot] on 2025-10-08 19:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-08 19:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
arviz (https://github.com/arviz-devs/arviz)
- arviz/tests/base_tests/test_stats_utils.py:226:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Unknown & ~str`, found `Unknown & ~Literal["any"]`
- arviz/tests/base_tests/test_stats_utils.py:228:82: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Unknown & ~str`, found `Unknown & ~Literal["any"]`
- Found 733 diagnostics
+ Found 731 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-10-09 11:30_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-09 11:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-09 11:30_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-09 11:30_

---

_@sharkdp reviewed on 2025-10-09 15:29_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2276 on 2025-10-09 15:29_

It's true that it is enough to test disjointness by checking if the intersection between the two top-materializations is empty, but I think I would prefer a definition that is a bit clearer / more intuitive.

Two gradual types A and B are disjoint if their intersection is empty: `A & B = Never`.

From this definition, you can arrive at your property:
* `A & B = Never` is true if and only if the bottom and the top materialization of the two types are the same, i.e. if `Top[A & B] = Never` and `Bottom[A & B] = Never`.
* `Top[X] = Never` always implies `Bottom[X] = Never`, because `Bottom[X] <: Top[X]`.
* This means that it is enough to test `Top[A & B] = Never`.
* Since `Top[…]` distributes over intersections, we can also write this as `Top[A] & Top[B] = Never`, which is equivalent to saying that the two fully-static types `Top[A]` and `Top[B]` must be disjoint.

---

_@AlexWaygood reviewed on 2025-10-09 15:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2276 on 2025-10-09 15:55_

> Two gradual types A and B are disjoint if their intersection is empty: `A & B = Never`.

Riiight, but with our current implementation, this is surely circular -- we call `is_disjoint_from` from our intersection simplification infrastructure to _determine_ whether `A & B` should simplify to `Never`!

---

_@sharkdp reviewed on 2025-10-09 17:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2276 on 2025-10-09 17:31_

Ok, I thought you wanted to clarify what the semantics of this function are, not how it's implemented (it's not implemented by comparing top-materializations either, as far as I can tell).

---

_@AlexWaygood reviewed on 2025-10-09 17:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2276 on 2025-10-09 17:40_

> it's not implemented by comparing top-materializations either

yes, fair point. I'll try to reword it 👍

---

_@sharkdp reviewed on 2025-10-09 18:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2276 on 2025-10-09 18:10_

I'm happy to review the rest of the PR as well, but that will have to wait until tomorrow :smile: 

---

_Comment by @sharkdp on 2025-10-10 07:38_

> Our implementation of assignability between a type `T` and another type `~U` is currently too strict. We currently only consider `T` assignable to `~U` if `Top[T]` is disjoint from `Top[U]`. That's correct for subtyping and redundancy, but not for assignability: for assignability, we should be more permissive, and allow `T` to be considered assignable to `~U` if `Bottom[T]` is disjoint from `Bottom[U]`.

Let me see if I understand this:

* `T` is a subtype of `~U` if `Top[T] <: Bottom[~U]`. We have `Bottom[~U] = ~Top[U]`, so this is equivalent to `Top[T] <: ~Top[U]`, which is equivalent to saying that `Top[T]` needs to be disjoint from `Top[U]` :heavy_check_mark: 
* `T` is assignable to `~U` if `Bottom[T] <: Top[~U]`, i.e. `Bottom[T] <: ~Bottom[U]`, which is equivalent to `Bottom[T]` being disjoint from `Bottom[U]` :heavy_check_mark: 
* `T` is redundant with `~U` if `Bottom[T] <: Bottom[~U]` (i.e. `Bottom[T] <: ~Top[U]`) and `Top[T] <: Top[~U]` (i.e. `Top[T] <: ~Bottom[T]`), which would mean that `Bottom[T]` needs to be disjoint from `Top[U]` and `Top[T]` needs to be disjoint from `Bottom[U]` :x: 

The check for redundancy implemented here is too strict. This is not too surprising, because we know that redundancy is a weaker relation than subtyping. A counterexample would be `T = Any` and `U = Any`. `T = Any` is not a subtype of `~U = ~Any = Any`, but the two types are redundant. And indeed, `Bottom[T] = Never` is disjoint from `Top[U] = object`, and `Top[T] = object` is disjoint from `Bottom[U] = Never` (the rules for redundancy above), but `Top[T] = object` is *not* disjoint from `Top[U] = object` (the rule for subtyping above).


I don't know what this means for this PR. I think there is still the bigger question as to whether or not we can actually use the true redundancy relation for union simplification (given that it breaks transitivity), or if we actually need something that sits between redundancy and subtyping, the sorts of ad-hoc rules that we currently implement in order to simplify things like `Any | Any` to `Any`.

---

_Comment by @AlexWaygood on 2025-10-10 10:15_

> * `T` is redundant with `~U` if `Bottom[T] <: Bottom[~U]` (i.e. `Bottom[T] <: ~Top[U]`) and `Top[T] <: Top[~U]` (i.e. `Top[T] <: ~Bottom[T]`), which would mean that `Bottom[T]` needs to be disjoint from `Top[U]` and `Top[T]` needs to be disjoint from `Bottom[U]` ❌
> 
> The check for redundancy implemented here is too strict. This is not too surprising, because we know that redundancy is a weaker relation than subtyping.

That all seems correct to me. It's also unchanged from the redundancy implementation on `main`, however. This PR only touches the assignability relation for intersections with negated elements; it doesn't touch the subtyping or redundancy relations. So I would argue that this PR is a strict improvement on the semantics implemented on `main`, even if there are still further improvements that we might be able to make to the redundancy implementation (which would require more tests, and would require us to double-check that there aren't examples we can think of where implementing the redundancy relation more fully in this way would break transitivity).

I could add a comment linking to this discussion, and noting that this is probably not a complete implementation of the redundancy relation, but that we always err on the side of strictness for redundancy?

> A counterexample would be `T = Any` and `U = Any`. `T = Any` is not a subtype of `~U = ~Any = Any`, but the two types are redundant. And indeed, `Bottom[T] = Never` is disjoint from `Top[U] = object`, and `Top[T] = object` is disjoint from `Bottom[U] = Never` (the rules for redundancy above), but `Top[T] = object` is _not_ disjoint from `Top[U] = object` (the rule for subtyping above).

Indeed. But pragmatically, we already recognise `Any` as being redundant from `~Any`, because of the fact that we eagerly simplify `~Any` to `Any` in our intersection builder: https://play.ty.dev/882fb732-dc66-4571-9898-ca51f0816cc0

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1734 on 2025-10-10 10:46_

I would probably modify this comment to reflect the findings here, but otherwise, this seems like a strict improvement over the behavior on `main` — I agree. Thank you!

---

_@sharkdp approved on 2025-10-10 10:46_

---

_Renamed from "[ty] Better implementation of type relations for intersections with negated gradual elements" to "[ty] Better implementation of assignability for intersections with negated gradual elements" by @AlexWaygood on 2025-10-10 11:07_

---

_Merged by @AlexWaygood on 2025-10-10 11:10_

---

_Closed by @AlexWaygood on 2025-10-10 11:10_

---

_Branch deleted on 2025-10-10 11:10_

---

_Comment by @carljm on 2025-10-10 16:18_

I think it is a "known issue" that our implementation of redundancy is "too strict" compared to what it theoretically could/should be, but it must be too strict in order to avoid breaking transitivity (and transitivity is critical to the correct functioning of union simplification.) 

> I think there is still the bigger question as to whether or not we can actually use the true redundancy relation for union simplification (given that it breaks transitivity), or if we actually need something that sits between redundancy and subtyping

I think this is basically right, but I would frame it slightly differently. The reason we named the redundancy relation "redundancy" is in order to communicate "this is the relation we use for determining redundancy in unions." So we don't "need something that sits between redundancy and subtyping." We already have exactly that: we call it "redundancy". It sits between "S+ <: T+ && S- <: T- subtyping" (what you are I think here calling "true redundancy", but we could also call "true subtyping") and "S+ <: T- subtyping" (which we currently call "subtyping", but could also be called "too-strict subtyping.")  Our redundancy relation is "too-strict subtyping", but with some ad-hoc elements of "true subtyping", (hopefully) carefully chosen to avoid breaking transitivity.

I don't think there is an open question about whether we can use a relation that breaks transitivity for union simplification. We definitely cannot, or else the result of union simplification depends on the order elements are added; this is not tenable.

---

_Comment by @AlexWaygood on 2025-10-10 16:21_

> I think this is basically right, but I would frame it slightly differently. The reason we named the redundancy relation "redundancy" is in order to communicate "this is the relation we use for determining redundancy in unions." So we don't "need something that sits between redundancy and subtyping." We already have exactly that: we call it "redundancy". It sits between "S+ <: T+ && S- <: T- subtyping" (what you are I think here calling "true redundancy", but we could also call "true subtyping") and "S+ <: T- subtyping" (which we currently call "subtyping", but could also be called "too-strict subtyping.") Our redundancy relation is "too-strict subtyping", but with some ad-hoc elements of "true subtyping", (hopefully) carefully chosen to avoid breaking transitivity.

Yes, I think the disconnect is that the ("pure"? But nontransitive?) version of the redundancy relation described in the doc-comment above the `TypeRelation::Redundancy` variant currently is not the version that we've actually implemented, because, as you say, the version that we've actually implemented deliberately errs on the side of strictness to avoid non-transitivity

---

_Comment by @carljm on 2025-10-10 16:23_

> I think the disconnect is that the ("pure"? But nontransitive?) version of the redundancy relation described in the doc-comment above the `TypeRelation::Redundancy` variant currently is not the version that we've actually implemented, because, as you say, the version that we've actually implemented deliberately errs on the side of strictness to avoid non-transitivity

Then we should reflect some of this discussion into that doc comment, to help us avoid continuing to have this same conversation on every PR related to redundancy 😆 

---

_Comment by @sharkdp on 2025-10-10 19:19_

I was genuinely confused by this, because there was an effort to formalize the new relation called redundancy in https://github.com/astral-sh/ruff/pull/20602, but then we ended up with a doc comment that essentially just describes "S+ <: T+ && S- <: T-" subtyping in a different way, as pointed out [here](https://github.com/astral-sh/ruff/pull/20602#discussion_r2405748633). So I thought we actually aim to implement the "S+ <: T+ && S- <: T-" subtyping relation under the name of "redundancy". But this discussion here clarifies the situation: the doc comment is wrong, and what we call "redundancy" is "too-strict subtyping, but with some ad-hoc elements of true subtyping".

In that case, I'm just worried that we won't be able to succinctly define what that relation actually is. Basically what Doug said [here](https://github.com/astral-sh/ruff/pull/20602#pullrequestreview-3280328751) (with what looks like a wrong definition of assignability :wink:).

---
