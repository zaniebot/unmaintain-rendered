```yaml
number: 21871
title: "[ty] Simplify union lower bounds and intersection upper bounds in constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/die-die-intersections
created_at: 2025-12-09T19:29:03Z
updated_at: 2025-12-10T15:54:57Z
url: https://github.com/astral-sh/ruff/pull/21871
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Simplify union lower bounds and intersection upper bounds in constraint sets

---

_@dcreager_

In a constraint set, it's not useful for an upper bound to be an intersection type, or for a lower bound to be a union type. Both of those can be rewritten as simpler BDDs:

```
T ≤ α & β  ⇒ (T ≤ α) ∧ (T ≤ β)
T ≤ α & ¬β ⇒ (T ≤ α) ∧ ¬(T ≤ β)
α | β ≤ T  ⇒ (α ≤ T) ∧ (β ≤ T)
```

We were seeing performance issues on #21551 when _not_ performing this simplification. For instance, `pandas` was producing some constraint sets involving intersections of 8-9 different types. Our sequent map calculation was timing out calculating all of the different permutations of those types:

```
t1 & t2 & t3 → t1
t1 & t2 & t3 → t2
t1 & t2 & t3 → t3
t1 & t2 & t3 → t1 & t2
t1 & t2 & t3 → t1 & t3
t1 & t2 & t3 → t2 & t3
```

(and then imagine what that looks like for 9 types instead of 3...)

With this change, all of those permutations are now encoded in the BDD structure itself, which is very good at simplifying that kind of thing.

Pulling this out of #21551 for separate review.

---

_Review requested from @carljm by @dcreager on 2025-12-09 19:29_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-09 19:29_

---

_Review requested from @sharkdp by @dcreager on 2025-12-09 19:29_

---

_Label `internal` added by @dcreager on 2025-12-09 19:29_

---

_Label `ty` added by @dcreager on 2025-12-09 19:29_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 19:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 19:33_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:721 on 2025-12-09 21:43_

Nit: would it make sense to check these conditions separately, immediately after constructing `lower` and `upper` respectively? Given that this is an OR, it seems like if `lower` does not simplify, there's no point in attempting to simplify `upper`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:2767 on 2025-12-09 21:51_

Trying to understand what's going on here and how it relates to the previous code. Previously we'd have gotten back a "simplified" constraint with union lower bound and/or intersection upper bound, and we'd have added the pair-implication and single-implications above. Now we just... do nothing? Can you add a comment giving a little explanation for why that's OK?

I guess maybe it's because we now never insert such a "complex intersected constraint" above, so we also don't need to include it in our sequent map?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:2172 on 2025-12-09 21:52_

And this is fine because it's always ok for `simplify` to do nothing.

---

_@carljm approved on 2025-12-09 21:52_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:721 on 2025-12-09 23:15_

My thinking was that the disjointness check should take precedence, though I'm having trouble coming up with a specific example where the ordering of the checks would matter.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2172 on 2025-12-09 23:16_

That, and also that if the intersection doesn't collapse to a single type, it's not really a simplification. Comment added

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2767 on 2025-12-09 23:21_

Right, the sequent map is trying to record which constraints might possibly appear in the BDD, and how they relate to each other. We don't need it to include constraints that we're never going to add to the BDD.

Added a comment

---

_@dcreager reviewed on 2025-12-09 23:22_

---

_@carljm reviewed on 2025-12-09 23:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:721 on 2025-12-09 23:24_

Ah, I didn't think about that, I think you're right that if we can identify disjointness we want to do that. I think that means not worth changing anything here.

---

_Merged by @dcreager on 2025-12-10 00:49_

---

_Closed by @dcreager on 2025-12-10 00:49_

---

_Branch deleted on 2025-12-10 00:49_

---

_@sharkdp reviewed on 2025-12-10 08:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:505 on 2025-12-10 08:55_

You write these as implications, but don't we need those to be equivalences?

The one about negations looks interesting. I guess it could be simplified to `T ≤ ¬β ⇒ ¬(T ≤ β)`, since the intersection part of it is already covered by the first rule. But more importantly, is this really correct? The left hand side seems to always be true for `T = Never`, whereas the right hand side seems to always be false for `T = Never`. That would mean it's not even true as an implication?

---

_@dcreager reviewed on 2025-12-10 14:38_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:505 on 2025-12-10 14:38_

> The one about negations looks interesting. I guess it could be simplified to `T ≤ ¬β ⇒ ¬(T ≤ β)`, since the intersection part of it is already covered by the first rule. But more importantly, is this really correct? The left hand side seems to always be true for `T = Never`, whereas the right hand side seems to always be false for `T = Never`. That would mean it's not even true as an implication?

Ah good catch! Negation doesn't distribute through the check like I wrote it. It should be something like

```
T ≤ ¬α & ¬β ⇒ (T ≤ ¬α) ∧ (T ≤ ¬β)
```

i.e. we can still separate out the negation elements of the intersection, but they should remain negated types and a positive ≤ check.

I'll fix this in a follow-on PR.

> You write these as implications, but don't we need those to be equivalences?

They are equivalences, but we're using this as a normalization step, so we only want to apply them in the direction that I've written them. That is, the goal with this change is that the upper bound of a constraint will never be an intersection type anymore. (and ditto for the lower bound never being a union type)

---

_@sharkdp reviewed on 2025-12-10 14:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:505 on 2025-12-10 14:47_



> >    You write these as implications, but don't we need those to be equivalences?
>
> They are equivalences, but we're using this as a normalization step, so we only want to apply them in the direction that I've written them. That is, the goal with this change is that the upper bound of a constraint will never be an intersection type anymore. (and ditto for the lower bound never being a union type)

:+1: 

I'm being pedantic, but I think what I meant was: we need these to be equivalences, or otherwise, the structural simplification that we apply here (in one direction) might lead to a constraint set that is not equivalent to the original constraint set anymore. But even if my thinking is correct, there's no need to change anything. The arrows can also just represent the direction in which we're performing the simplification. I mainly wanted to understand.

---

_@dcreager reviewed on 2025-12-10 15:54_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:505 on 2025-12-10 15:54_

> we need these to be equivalences, or otherwise, the structural simplification that we apply here (in one direction) might lead to a constraint set that is not equivalent to the original constraint set anymore.

Got it! I think I got this meaning correct in the new comment in https://github.com/astral-sh/ruff/pull/21897

---
