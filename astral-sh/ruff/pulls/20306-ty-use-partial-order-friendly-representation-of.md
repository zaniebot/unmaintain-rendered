```yaml
number: 20306
title: "[ty] Use partial-order-friendly representation of typevar constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/incomparable
created_at: 2025-09-08T15:03:27Z
updated_at: 2025-09-09T19:54:49Z
url: https://github.com/astral-sh/ruff/pull/20306
synced_at: 2026-01-12T15:56:58Z
```

# [ty] Use partial-order-friendly representation of typevar constraints

---

_@dcreager_

The constraint representation that we added in #19997 was subtly wrong, in that it didn't correctly model that type assignability is a _partial_ order — it's possible for two types to be incomparable, with neither a subtype of the other. That means the negation of a constraint like `T ≤ t` (typevar `T` must be a subtype of `t`) is **_not_** `t < T`, but rather `t < T ∨ T ≁ t` (using ≁ to mean "not comparable to").

That means we need to update our constraint representation to be an enum, so that we can track both _range_ constraints (upper/lower bound on the typevar), and these new _incomparable_ constraints.

Since we need an enum now, that also lets us simplify how we were modeling range constraints. Before, we let the lower/upper bounds be either open (<) or closed (≤). Now, range constraints are always closed, and we add a third kind of constraint for _not equivalent_ (≠). We can translate an open upper bound `T < t` into `T ≤ t ∧ T ≠ t`.

We already had the logic for doing adding _clauses_ to a _set_ by doing a pairwise simplification. We copy that over to where we add _constraints_ to a _clause_. To calculate the intersection or union of two constraints, the new enum representation makes it easy to break down all of the possibilities into a small number of cases: intersect range with range, intersect range with not-equivalent, etc. I've done the math [here](https://dcreager.net/theory/constraints/) to show that the simplifications for each of these cases is correct.

---

_Review requested from @carljm by @dcreager on 2025-09-08 15:03_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-08 15:03_

---

_Review requested from @sharkdp by @dcreager on 2025-09-08 15:03_

---

_Label `internal` added by @dcreager on 2025-09-08 15:03_

---

_Label `ty` added by @dcreager on 2025-09-08 15:03_

---

_Comment by @github-actions[bot] on 2025-09-08 15:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-08 15:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:90 on 2025-09-08 21:32_

Doesn't this exposition still ignore the possibility that `V` is not comparable to `B` (or to `Never`, though in the particular case of `Never` that's impossible, all types are comparable to the bottom type by definition.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:478 on 2025-09-08 21:44_

There are still many references to "atomic constraint" in doc comments, here and throughout (including on methods of `ConstrainedTypeVar` itself.) Is this language you want to keep using? I don't have a better idea, but maybe at some point it should be clarified that by "atomic constraint" you mean "ConstrainedTypeVar"?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1062 on 2025-09-08 21:58_

I think more can be checked here? If we have the range constraint `L <: T <: U` and the not-comparable constraint `T nc X` (is there a standard symbol for not-comparable? doesn't seem like it), then if `U <: X` or `X <: L`, by transitivity of subtyping `T <: X` or `X <: T`, meaning the intersection is never satisfiable.

IOW I think the `is_equivalent_to` checks here should be `is_subtype_of` checks instead.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1034 on 2025-09-08 22:01_

What about the case where `other.ty` is neither a supertype of `self.lower` nor a subtype of `self.upper` -- therefore it's outside the range, and the intersection can simplify to just the range?

---

_@carljm approved on 2025-09-08 22:10_

Looks reasonable!

I'm sure if I tell you that all this math seems well-suited to a nice unit test suite, it won't be the first you've thought of it ;)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-08 22:12_

---

_@dcreager reviewed on 2025-09-09 13:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1062 on 2025-09-09 13:12_

> is there a standard symbol for not-comparable? doesn't seem like it

I've been using ≁ (`\u2241`). I have a pretty [heavily customized](https://github.com/dcreager/dotfiles-public/blob/d98071250a4328504c6b6ed3275c157608b5e535/.XCompose.symlink#L25) Compose key on my linux boxes; not sure how to type that ergonomically on macos

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1062 on 2025-09-09 13:26_

Yes, good catch! This was bad copy/pasta from the method above, I had subtype checks as you suggest when I did this in the math! [[lower bound](https://dcreager.net/theory/constraints/intersect-lower-ncmp/), [upper bound](https://dcreager.net/theory/constraints/intersect-upper-ncmp/)]

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1034 on 2025-09-09 13:55_

Nice! It [actually works](https://dcreager.net/theory/constraints/intersect-range-neq/#Case-2) if _either_ `L ⊈ X` or `X ⊈ U`. Updated

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:478 on 2025-09-09 14:03_

Reworded to use "individual constraint", which I was using in a couple of places already

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:90 on 2025-09-09 14:03_

Yes, good catch! I had updated this to remove the open/closed distinction.  Added text about incomparable

---

_@dcreager reviewed on 2025-09-09 14:07_

---

_Comment by @dcreager on 2025-09-09 18:11_

> I'm sure if I tell you that all this math seems well-suited to a nice unit test suite, it won't be the first you've thought of it ;)

I'm tackling this in a separate PR, https://github.com/astral-sh/ruff/pull/20319

---

_@carljm approved on 2025-09-09 18:40_

🚀 

---

_Merged by @dcreager on 2025-09-09 19:54_

---

_Closed by @dcreager on 2025-09-09 19:54_

---

_Branch deleted on 2025-09-09 19:54_

---
