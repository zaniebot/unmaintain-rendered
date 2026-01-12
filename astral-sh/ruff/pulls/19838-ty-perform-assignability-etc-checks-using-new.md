```yaml
number: 19838
title: "[ty] Perform assignability etc checks using new `Constraints` trait"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/relation-with-constraints
created_at: 2025-08-08T20:59:14Z
updated_at: 2025-08-21T13:30:11Z
url: https://github.com/astral-sh/ruff/pull/19838
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Perform assignability etc checks using new `Constraints` trait

---

_@dcreager_

"Why would you do this? This looks like you just replaced `bool` with an overly complex trait"

Yes that's correct!

This should be a no-op refactoring. It replaces all of the logic in our assignability, subtyping, equivalence, and disjointness methods to work over an arbitrary `Constraints` trait instead of only working on `bool`.

The methods that `Constraints` provides looks very much like what we get from `bool`. But soon we will add a new impl of this trait, and some new methods, that let us express "fuzzy" constraints that aren't always true or false. (In particular, a constraint will express the upper and lower bounds of the allowed specializations of a typevar.)

Even once we have that, most of the operations that we perform on constraint sets will be the usual boolean operations, just on sets. (`false` becomes empty/never; `true` becomes universe/always; `or` becomes union; `and` becomes intersection; `not` becomes negation.) So it's helpful to have this separate PR to refactor how we invoke those operations without introducing the new functionality yet.

Note that we also have translations of `Option::is_some_and` and `is_none_or`, and of `Iterator::any` and `all`, and that the `and`, `or`, `when_any`, and `when_all` methods are meant to short-circuit, just like the corresponding boolean operations. For constraint sets, that depends on being able to implement the `is_always` and `is_never` trait methods.

---

_Comment by @github-actions[bot] on 2025-08-08 21:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 21:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-08 21:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Frelation-with-constraints?runnerMode=WallTime)

### Merging #19838 will **not alter performance**

<sub>Comparing <code>dcreager/relation-with-constraints</code> (f10babf) with <code>main</code> (045cba3)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Label `ty` added by @AlexWaygood on 2025-08-08 22:09_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:510 on 2025-08-19 18:37_

This is a common pattern in this PR, to implement short-circuiting when we can't (easily) use `when_all` or `when_any`. This method is (and was) a large intersection. Before, we'd use early `return false` to implement the short-circuiting. With `bool`, if any particular element doesn't short-circuit, there's only one possible "current pending result" — `true`. So we didn't need to explicitly track it, and down at the end of the method, where we know made it all the way through without ever getting a short-circuited `false`, we can return `true` directly.

But with constraint sets, we have to create a `result` accumulator, and explicitly intersect each element's constraints into the result, since we might end up with something that is neither "always true" nor "always false". I've structured the return values of `intersect` and `union` so that the check always has the form

```rust
if short_circuit {
    return result;
}
```

---

_Marked ready for review by @dcreager on 2025-08-19 18:38_

---

_Review requested from @carljm by @dcreager on 2025-08-19 18:38_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-19 18:38_

---

_Review requested from @sharkdp by @dcreager on 2025-08-19 18:38_

---

_@dcreager reviewed on 2025-08-19 18:38_

---

_Renamed from "[ty] WIP: Perform assignability etc checks using new `Constraints` trait" to "[ty] Perform assignability etc checks using new `Constraints` trait" by @dcreager on 2025-08-19 19:05_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1495 on 2025-08-20 23:40_

I'm curious how you think the logic can be simplified "once we're using real constraint sets"? To me it appears kind of the opposite -- this duplication is here precisely because we are trying to be correct in a future with real constraint sets, where we need to return the actual accumulated constraints from the inner `has_relation_to_impl` checks. In this PR, with just bool, we can pass all tests without this duplication, by just changing the sense of the match arm `if` clause back to a positive `.is_always(db)` instead of a negated `.is_never(db)`, and making the body of the match arm be `C::always(db)`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1495 on 2025-08-20 23:47_

Considering this case is actually making me wonder about a more fundamental thing: what about a case where this match arm returns some constraints (neither always nor never), but some later match arm would also have returned some constraints, had we reached it? In an ideal world, wouldn't we OR those constraint sets, if we're trying to express the conditions under which the relation would be true?

I guess in the real world, we just pick one of them and don't find all solutions?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1686 on 2025-08-20 23:50_

I guess the change in semantics is safe here because the very next match arm unconditionally returns false/never for `(Type::Callable(_), _)`

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1338 on 2025-08-21 00:02_

Do you foresee us needing this genericity over `C: Constraints` long-term? Or is this just needed for smoothing the transition from bool to real constraint sets, and once we've done that, these methods will in practice be monomorphic over just that "constraint sets" implementation of the trait?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 00:08_

I find the return value of `intersect` counter-intuitive here. IIUC this is saying "if this intersection is empty" but it looks like it might be saying "if there is a non-empty intersection".

I think I would find it clearer if `intersect` just returned `self` and we had to explicitly call `is_never()` on it -- but maybe that makes other short-circuiting cases more painful?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:8 on 2025-08-21 00:10_

There's the obvious name tension with `Type::Never` here, but I'm not sure I have a better name to offer.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:153 on 2025-08-21 00:13_

If these two cases are what we mean when we say above that we use the return types of `intersect` and `union` for short-circuiting, then my opinion in my other comment is unchanged: I think it would be clearer, and not particularly painful, for `union` and `intersect` to return `self` and require an explicit chained call to `.is_always()` or `.is_never()` if you want to use those methods in boolean context.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:128 on 2025-08-21 00:15_

Thanks for also increasing our visitor coverage in this diff!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:510 on 2025-08-21 00:21_

Aha, and these cases are why you want `union` and `intersect` to return a somewhat-counter-intuitive boolean? But even here, it looks like you'd only need to call `is_never()` in one place, inside `types_inconsistent`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:511 on 2025-08-21 00:22_

Not sure I see a good reason for the rename from `check_types` to `types_inconsistent` (with the reversed sense). I usually think negatives in method names should be avoided, and `if types_inconsistent(...)` doesn't seem better to me than `if !check_types(...)`

---

_@carljm approved on 2025-08-21 00:24_

This is quite cool! Very impressed with how you wrangled the Rust API to make the translation fairly mechanical in many cases, even though you're adding significant capability.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 00:41_

> I think I would find it clearer if `intersect` just returned `self` and we had to explicitly call `is_never()` on it -- but maybe that makes other short-circuiting cases more painful?

It would need to both update `self` in-place (since it's an accumulator) and also return a clone that the caller would then call `is_never` on.  I thought that could be inefficient once we have non-`bool` implementations, and that it would be better for this to return a `bool`.

Maybe if I rename the method to `intersect_short_circuits`, so that it's clearer at the call site what the return value represents?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:510 on 2025-08-21 00:42_

> Aha, and these cases are why you want `union` and `intersect` to return a somewhat-counter-intuitive boolean?

Yep, exactly

> But even here, it looks like you'd only need to call `is_never()` in one place, inside `types_inconsistent`.

That's true for this case, but there are other places where we're doing the short circuit check several times in a single method, so I wanted it to be as ergonomic as possible.

(But more importantly, the efficiency bit that I mention in the other comment thread)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1686 on 2025-08-21 00:43_

Yes that's right, and I wanted to avoid the repetition like I have up in the typevar arm

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1338 on 2025-08-21 00:45_

Originally I thought that we would eventually be able to remove the trait. But in Discord @AlexWaygood asked about whether this PR's change would be compatible with returning a `Result` from `has_relation_to`, etc. I think we could do that by implementing the trait for that `Result` type, too (possibly wrapping the impl of the `T` inside the result). So...I'm not sure? Hopefully but maybe not.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:8 on 2025-08-21 00:45_

`unsatisfiable` or `never_satisfiable`, but that's longer to type :smile: 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:511 on 2025-08-21 00:47_

This was an attempt to be consistent that everywhere that does a short-circuit check has the same syntactic form, with a positive `if` condition check.  Maybe `type_check_short_circuits`, similar to my other rename suggestion, to keep the positive `if` condition but so that there's no negative in the name?

---

_@dcreager reviewed on 2025-08-21 00:47_

---

_@carljm reviewed on 2025-08-21 00:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 00:52_

Can't it return `&Self` without needing a clone?

---

_@dcreager reviewed on 2025-08-21 01:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 01:12_

Hmm, maybe, I can try that.  Does that work if the receiver is `&mut self`?

---

_@dcreager reviewed on 2025-08-21 01:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1495 on 2025-08-21 01:17_

My thinking is not that the stuff inside the match arm would change all that much. The part that I find too complex in its current state is that so much depends on the _order_ of the match arms. My hunch is that the fall-through here won't be needed, and the ordering will no longer matter as much, because we'll be able to better combine the results of each recursive call with the other surrounding constraints.

---

_@dcreager reviewed on 2025-08-21 01:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 01:19_

> Does that work if the receiver is `&mut self`?

Looks like [it does](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=1aeb86e1df6189c64731585587673f9d)!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:510 on 2025-08-21 01:34_

These now return more-intuitive values

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:621 on 2025-08-21 01:34_

Updated to return `&Self`, and to use explicit `is_never` and `is_always` calls at the call sites

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:153 on 2025-08-21 01:34_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:511 on 2025-08-21 01:36_

The name change is reverted back to `check_types`.  I also opted to keep the meaning of the returned boolean be the same, so the call sites are back to being `!check_types`, as before. That means that there's technically a double-negation happening, but that doesn't bother me.

---

_@dcreager reviewed on 2025-08-21 01:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:19 on 2025-08-21 04:33_

```suggestion
    /// Updates this constraint set to hold the union of itself and another constraint set.
```

---

_@carljm approved on 2025-08-21 04:34_

Looks great!

---

_@AlexWaygood reviewed on 2025-08-21 11:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:8 on 2025-08-21 11:45_

> `unsatisfiable` or `never_satisfiable`, but that's longer to type 😄

I sort-of prefer those, actually! `unsatisfiable` doesn't seem too verbose to me. Not a big deal though.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1 on 2025-08-21 11:52_

It doesn't have to be added in this PR, but I'd love some more documentation about what a constraints set is exactly, with some examples about when it might matter that we return a constraints set rather than a `bool` from `has_relation_to_impl`. As it is, I think I'd feel very nervous editing this file if I needed to make updates for a PR of my own; I don't feel like I have a confident enough understanding of everything that's going on here

---

_@AlexWaygood reviewed on 2025-08-21 11:52_

---

_@dcreager reviewed on 2025-08-21 12:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:8 on 2025-08-21 12:12_

No problem, it's just a couple of `sed` commands to make the change!

For the predicates, I've gone with `is_{always,never}_satisfied`. For the constructors, I want them to be consistent, so I went with `unsatisfied`/`always_satisfied`.

---

_@dcreager reviewed on 2025-08-21 12:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1 on 2025-08-21 12:19_

Done! 

> when it might matter that we return a constraints set rather than a `bool` from `has_relation_to_impl`

This part relates to @carljm's question above https://github.com/astral-sh/ruff/pull/19838#discussion_r2289557108, about whether the trait will stick around long-term or not. My goal is that the `bool` impl will go away soon (likely in the very next PR https://github.com/astral-sh/ruff/pull/19997)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1 on 2025-08-21 12:30_

Thank you, that module doc-comment is exactly what I was looking for!

---

_@AlexWaygood reviewed on 2025-08-21 12:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:4 on 2025-08-21 12:34_

```suggestion
//! simple answers: one type is either assignable to another type, or it isn't. (The _rules_ for
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:12 on 2025-08-21 12:35_

```suggestion
//! hold for some specializations, but not for others. That means that for types that include
//! typevars, "Is this type assignable to another?" no longer makes sense as a question. The better
//! question is: "Under what constraints is this type assignable to another?".
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:15 on 2025-08-21 12:36_

```suggestion
//! question. An individual constraint restricts the specialization of a single typevar to be within a
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:17 on 2025-08-21 12:36_

```suggestion
//! union, intersection, and negation operations (just like types themselves).
```

---

_@AlexWaygood approved on 2025-08-21 12:37_

---

_Merged by @dcreager on 2025-08-21 13:30_

---

_Closed by @dcreager on 2025-08-21 13:30_

---

_Branch deleted on 2025-08-21 13:30_

---
