```yaml
number: 20606
title: "[ty] `~T` should never be assignable to `T`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/negated-typevar-intersection
created_at: 2025-09-28T12:14:41Z
updated_at: 2025-10-02T06:52:50Z
url: https://github.com/astral-sh/ruff/pull/20606
synced_at: 2026-01-10T17:40:28Z
```

# [ty] `~T` should never be assignable to `T`

---

_Pull request opened by @AlexWaygood on 2025-09-28 12:14_

## Summary

Currently we do not emit an error on this code:

```py
from ty_extensions import Not

def f[T](x: T, y: Not[T]) -> T:
    x = y
    return x
```

But we should do! `~T` should never be assignable to `T`.

This fixes a small regression introduced in https://github.com/astral-sh/ruff/commit/14fe1228e72dca90db6b3dbb7e07f973d175ea0e#diff-8049ab5af787dba29daa389bbe2b691560c15461ef536f122b1beab112a4b48aR1443-R1446, where a branch that previously returned `false` was replaced with a branch that returns `C::always_satisfiable` -- the opposite of what it used to be! The regression occurred because we didn't have any tests for this -- so I added some tests in this PR that fail on `main`. I only spotted the problem because I was going through the code of `has_relation_to_impl` with a fine toothcomb for https://github.com/astral-sh/ruff/pull/20602 😄

---

_Label `ty` added by @AlexWaygood on 2025-09-28 12:14_

---

_Comment by @github-actions[bot] on 2025-09-28 12:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-28 12:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-09-28 12:23_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-28 12:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-28 12:23_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-28 12:23_

---

_@sharkdp reviewed on 2025-09-29 08:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-09-29 08:19_

I'm slightly surprised that we need these branches at all. Shouldn't the union/intersection branches take care of this, if `T <: T` is modeled correctly?

Also, is the statement "`~T` is never assignable to `T`" correct if `T` could specialize to `Any`? Or do we not have to take this case into account, for some reason?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-09-29 11:03_

> I'm slightly surprised that we need these branches at all. Shouldn't the union/intersection branches take care of this, if `T <: T` is modeled correctly?

Yes -- ideally we'd have the `NonInferrableTypeVar` branches below the `Union` and `Intersection` branches. Then we wouldn't need these branches at all, as you say. Unfortunately, however, value-constrained TypeVars make this quite hard, as they require special handling with regards to unions:

https://github.com/astral-sh/ruff/blob/3f640dacd42666f83d1a36d29efc507d31116060/crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md?plain=1#L227-L257

> Also, is the statement "`~T` is never assignable to `T`" correct if `T` could specialize to `Any`? Or do we not have to take this case into account, for some reason?

Hmm, interesting question. I think that also calls into doubt the validity of this pre-existing branch -- if the TypeVar could be solved to `Any`, it should not be considered a subtype of itself:

https://github.com/astral-sh/ruff/blob/3f640dacd42666f83d1a36d29efc507d31116060/crates/ty_python_semantic/src/types.rs#L1568-L1576

I wonder if this is something that @dcreager's work on constraint sets could help with? If the relation is `TypeRelation::Subtyping`, we could possibly return a constraint set from these branches that indicates that `self` is a subtype of `target` only if neither `self` nor `target` is solved to `Any`?

---

_@AlexWaygood reviewed on 2025-09-29 11:03_

---

_@AlexWaygood reviewed on 2025-09-29 11:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-09-29 11:31_

> I wonder if this is something that @dcreager's work on constraint sets could help with? If the relation is `TypeRelation::Subtyping`, we could possibly return a constraint set from these branches that indicates that `self` is a subtype of `target` only if neither `self` nor `target` is solved to `Any`?

But that doesn't really make sense, of course, because these are _non-inferable_ type variables -- type variables from the perspective of inside a function or class body... I think this does lead to the conclusion that `T` cannot be a subtype of `T` (where `T` is a non-inferable type variable); it can only be assignable to `T`. No matter what the bounds or constraints on a type variable are, it could _always_ be specialised to `Any`, so you can't really ever conclude that a non-inferable type variable is ever a subtype of (or disjoint from) anything, I don't think

---

_@carljm reviewed on 2025-09-29 13:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-09-29 13:57_

I don't think we need to account for the possibility that a typevar is specialized to Any here. The key is that it's the same typevar, so it would have to be specialized to the _same_ Any in both usages -- which is really the same as saying it's specialized to some unknown type (which is already what we were accounting for). But since it's the _same_ type for both occurrences of T, the conclusions here still apply. 

---

_@AlexWaygood reviewed on 2025-09-29 14:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-09-29 14:02_

Right, I think that makes sense

---

_@AlexWaygood reviewed on 2025-10-01 17:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-10-01 17:22_

so... the conclusion from Carl's comment is that the logic in this PR is correct?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1565 on 2025-10-02 01:11_

> I don't think we need to account for the possibility that a typevar is specialized to Any here. The key is that it's the same typevar, so it would have to be specialized to the _same_ Any in both usages

Right, I'd even reword this slightly to say that typevar _inference_ will never infer a gradual type for a typevar. (A typevar can be specialized to `Unknown` if we don't infer _anything_ for it, and it has no default, but I consider that different than explicitly infering a gradual type for it.)

> I'm slightly surprised that we need these branches at all. Shouldn't the union/intersection branches take care of this, if `T <: T` is modeled correctly?

The current way we're handling assignability of typevars is very brittle and dependent on these kinds of hacks, because of "rounding error" that's introduced by the early conversions of the assignability checks of union/intersection elements into a true/false value. https://github.com/astral-sh/ruff/pull/20093 is working on using constraint sets to model the assignability of a typevar, which will allow us to correctly model when e.g. each element of a union is assignable "sometimes", but in a way that combines to "always" when you merge them together. That will hopefully remove the need for these match arms completely. But having the mdtest here will be good to avoid a regression when that lands.

---

_@dcreager approved on 2025-10-02 01:11_

---

_Merged by @AlexWaygood on 2025-10-02 06:52_

---

_Closed by @AlexWaygood on 2025-10-02 06:52_

---

_Branch deleted on 2025-10-02 06:52_

---
