```yaml
number: 20651
title: "[ty] Fix simplification of `T & ~T` for non-fully-static types"
type: pull_request
state: open
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: alex/non-fully-static-negations
created_at: 2025-09-30T16:48:25Z
updated_at: 2025-10-03T12:02:02Z
url: https://github.com/astral-sh/ruff/pull/20651
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Fix simplification of `T & ~T` for non-fully-static types

---

_@AlexWaygood_

## Summary

~~(Stacked on top of https://github.com/astral-sh/ruff/pull/20650; review that PR first!)~~

On `main`, we simplify `Any & ~Any` to `Any`, but we simplify `list[Any] & ~list[Any]` to `Never`. This is inconsistent! The justification for the first simplification applies to all non-fully-static types, so we should apply the same treatment to both.

`T & ~T` cannot simplify to `Never` for any non-fully-static type `T`, but it can simplify to `T`. We cannot say that `list[Any] & ~list[Any]` is so small such that the only materialization of the type is `Never`, as the first `Any` might (for example) materialize to `int` while the second one might materialize to `bool`, and `list[int] & ~list[bool]` is not equivalent to `Never` (it's equivalent to `list[int]`, since the two types are disjoint)

## Test Plan

Added mdtests that fail on `main`


---

_Review requested from @carljm by @AlexWaygood on 2025-09-30 16:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-30 16:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-30 16:48_

---

_Label `bug` added by @AlexWaygood on 2025-09-30 16:48_

---

_Label `ty` added by @AlexWaygood on 2025-09-30 16:48_

---

_Comment by @github-actions[bot] on 2025-09-30 16:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-30 16:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-10-01 08:41_

> On `main`, we simplify `Any & ~Any` to `Any`, but we simplify `list[Any] & ~list[Any]` to `Never`. This is inconsistent!

I agree

> The justification for the first simplification applies to all non-fully-static types, so we should apply the same treatment to both.

Which justification?

> `T & ~T` cannot simplify to `Never` for any non-fully-static type `T`, but it can simplify to `T`.

I don't think so? That's not even true for fully-static types. From my calculations, `T & ~T` should simplify to **`(Top[T] & ~Bottom[T]) & Any`**, which is only equivalent to `T` if `Bottom[T] = Never`.

* For fully static types `T`, `Top[T]` is equivalent to `Bottom[T]`, and so `Top[T] & ~Bottom[T] = Never`, which makes the whole `T & ~T` expression equivalent to `Never`, as expected.
* For `T = Any`, we have `Top[T] = object` and `Bottom[T] = Never`. And so `(Top[T] & ~Bottom[T]) & Any = (object & ~Never) & Any = Any`, as expected.
* For a gradual type like `T = Any & int` with a bottom materialization of `Never`, we indeed get `T & ~T = (int & ~Never) & Any = int & Any = T`
* But for a gradual type like `T = Any | int`, this is not true. Here, we get `T & ~T = (object & ~int) & Any = ~int & Any`, which is not equivalent to `Any | int`.

---

_Comment by @AlexWaygood on 2025-10-01 17:17_

> > The justification for the first simplification applies to all non-fully-static types, so we should apply the same treatment to both.
> 
> Which justification?

Apologies, I linked to this on Discord but should have put this in my PR description, and probably also have added better comments alongside the code changes in this PR. I was referring to @carljm's justification [here](https://github.com/astral-sh/ruff/pull/13775#discussion_r1805636567):

> I think, however, that [`Any & ~Any`] should simplify to just `Any`. Adding `Any`, `Unknown`, or `Todo` to the negative side of an intersection should always just add them to the positive side of the intersection instead. The two are equivalent: "must not be in some unknown set of values" is equivalent to "must be in some unknown set of values"; the one "unknown set of values" is just the complement of the other. Since it's an unknown set anyway, we are free to replace it with its complement.

In the same way, we can say that `list[Any]` means "must be in some unknown set of values that is no larger than `Top[list[Any]]`" and `~list[Any]` means "must not be in some unknown set of values that is no larger than `Top[list[Any]]`". Since the two unknown sets have the same upper bound and the same lower bound, we are free to replace `list[Any] & ~list[Any]` with `list[Any]` in the same way that we replace `Any & ~Any` with `Any`. The fact that some unknown set of values must be subtracted from the set of possible values doesn't contribute anything meaningful to our understanding of the type when that unknown set has the same upper and lower bound as the positive contributions to the intersection.

> From my calculations, `T & ~T` should simplify to **`(Top[T] & ~Bottom[T]) & Any`**, which is only equivalent to `T` if `Bottom[T] = Never`.

The "intersect with `Any`" part of this surprises me! Where did you derive this formulation from? But...

> But for a gradual type like `T = Any | int`, this is not true. Here, we get `T & ~T = (object & ~int) & Any = ~int & Any`, which is not equivalent to `Any | int`.

the conclusion here definitely seems correct to me. If `Any` on the r.h.s. materialized to `Never`, we'd get `(int | Any) & ~(int | Never)` -> `(int | Any) & ~int` -> `(int & ~int) | (Any & ~int)` -> `Never | (Any & ~int)` -> `Any`. And if `Any` on the r.h.s. materialized to `object`, we'd get `(int | Any) & ~(int | object)` -> `(int | Any) & ~object` -> `Never`. The union of the two is `(Any & ~int) | Never` -> `Any & ~int`.

Thinking through the formulation like that makes me wonder if we can simply say that `T & ~U` should always be equivalent to `(T & ~Top[U]) | (T & ~Bottom[U])`? That feels more appealing to me than "randomly" intersecting with `Any` 😄. Working through some other test cases:
- Fully static types: `int & ~int` -> `(int & ~Top[int]) | (int & ~Bottom[int])` -> (int & ~int) | (int & ~int)` -> `Never | Never` -> `Never`
- `Any` itself: `Any & ~Any` -> `(Any & ~Top[Any]) | (Any & ~Bottom[Any])` -> `(Any & ~object) | (Any & ~Never)` -> `Never | Any` -> `Any`
- invariant generics specialized by `Any`: `list[Any] & ~list[Any]` -> `(list[Any] & ~Top[list[Any]]) | (list[Any] & ~Bottom[list[Any]])` -> ???
- intersections with `Any` contributions: `(Any & int) & ~(Any & int)` -> `(Any & int & ~Top[Any & int]) | (Any & int & ~Bottom[Any & int])` -> `(Any & int & ~int) | (Any & int & ~Never)` -> `Never | (Any & int)` -> `Any & int`

---

_Comment by @sharkdp on 2025-10-02 09:30_

> "must not be in some unknown set of values" is equivalent to "must be in some unknown set of values"

I like these sort of phrases as a justification after the fact, but I don't think they are good tools for a proof.

> The "intersect with `Any`" part of this surprises me! Where did you derive this formulation from?

The key technique for any of these calculations is to write gradual types in the form `T = Top[T] & Any | Bottom[T]` with `Bottom[T] <: Top[T]` the two fully-static types that completely characterize the gradual type `T`. The fact that this can be done for any gradual type is non-trivial (see https://www.irif.fr/~gc/papers/set-theoretic-types-2022.pdf, p. 53, *"Given a gradual type τ, the set of all static types it materializes to forms a complete lattice, with a maximum and a minimum static type that we denote by τ⇑ and τ⇓, respectively."*).

But once we have this representation, we can start to do useful calculations. For example, we can rewrite `~T` to resemble the universal form:

```
~T = ~(Top[T] & Any | Bottom[T])
   = ~(Top[T] & Any) & ~Bottom[T]
   = (~Top[T] | ~Any) & ~Bottom[T]
   = ~Top[T] & ~Bottom[T] | ~Any & ~Bottom[T]
   = ~Top[T] | Any & ~Bottom[T]
   = ~Bottom[T] & Any | ~Top[T]
```

And from this representation, we can extract that `Top[~T] = ~Bottom[T]` and `Bottom[~T] = ~Top[T]`. In a similar way, we can prove that `Top[…]` and `Bottom[…]` distribute over intersections (and unions), i.e. `Top[A & B] = Top[A] & Top[B]`. Once we have that, we can compute:

```
Top[T & ~T] = Top[T] & Top[~T] = Top[T] & ~Bottom[T]
Bottom[T & ~T] = Bottom[T] & Bottom[~T] = Bottom[T] & ~Top[T] = Never
```

And finally we can rewrite this in the universal form

```
T & ~T = Top[T & ~T] & Any | Bottom[T & ~T]
       = (Top[T] & ~Bottom[T]) & Any | Never
       = (Top[T] & ~Bottom[T]) & Any
```

What this type shows is that `Never` is *always* a possible materialization of `T & ~T`. But it can also be as "fuzzy" as `Any` (if `T` itself is `Any`). If a gradual type has a bottom materialization of `Never`, then the expression above simplifies to `(Top[T] & ~Bottom[T]) & Any = (Top[T] & object) & Any = Top[T] & Any` which is equivalent to `T`. So whenever the bottom materialization of a type is `Never`, `T & ~T = T`.

By the way, we could also compute `T | ~T = Any | (Bottom[T] | ~Top[T])`, which demonstrates the usual duality between intersections and unions. Here, `object` is *always* a possible materialization, but it can be as fuzzy as `Any` (if `T` itself is `Any`).

---

_Comment by @sharkdp on 2025-10-02 09:38_

> Thinking through the formulation like that makes me wonder if we can simply say that `T & ~U` should always be equivalent to `(T & ~Top[U]) | (T & ~Bottom[U])`?

That doesn't look correct. Take `T = object` and `U = Any`. `T & ~U = object & ~Any = object & Any = Any`. But your expression gives `(object & Never) | (object & object) = object`.

I think it should be `T & ~U = (T & ~Top[U]) | (T & ~Bottom[U] & Any)`. The `& Any` was missing.

---

_Converted to draft by @AlexWaygood on 2025-10-03 12:02_

---
