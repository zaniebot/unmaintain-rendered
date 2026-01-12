```yaml
number: 15738
title: "[red-knot] Decompose `bool` to `Literal[True, False]` in unions and intersections"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/truthy-unions-5
created_at: 2025-01-25T13:10:29Z
updated_at: 2025-04-28T07:11:52Z
url: https://github.com/astral-sh/ruff/pull/15738
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Decompose `bool` to `Literal[True, False]` in unions and intersections

---

_@AlexWaygood_

## Summary

Our union builder currently has a problem due to a bad interaction of several features we've implemented:
- The union builder understands that `bool ≡ Literal[True, False]` (and therefore that for any `T`, `bool | T ≡ Literal[True, False] | T`; therefore, whenever it sees `Literal[True, False]` in a union, it eagerly simplifies this union to the simpler type `bool`.
- The union builder also understands that adding `U` to the union `S | T` is a no-op if `U` is a subtype of either `S` or `T`
- The union builder understands that `Literal[True]` is a subtype of `AlwaysTruthy` and `~AlwaysFalsy`
- The union builder understands that `Literal[Falsy]` is a subtype of `AlwaysFalsy` and `~AlwaysTruthy`
- The union builder understands that `bool` is neither a subtype of `AlwaysTruthy` nor a subtype of `AlwaysFalsy`

Putting all these features together means that the union builder can produce different unions depending on the order in which elements are inserted into the union. Consider a union consisting of `Literal[True]`, `Literal[False]` and `AlwaysTruthy`. If they're inserted in that order, then the unions is built like this:

```
(1) `Literal[True]`
(2) add `Literal[False]` to the union:
    -> `Literal[True] | Literal[False]`
    -> eagerly simplified to `bool`
(3) add `AlwaysTruthy` to the union:
    -> `bool | AlwaysTruthy`

Final union: `bool | AlwaysTruthy`
```

But if they're inserted in a slightly different order, a different union is constructed entirely, which red-knot is not capable of understanding to be equivalent to the one above:

```
(1) `Literal[False]
(2) add `AlwaysTruthy` to the union
    -> `Literal[False] | AlwaysTruthy`
(3) add `Literal[True]` to the union
    -> still `Literal[False] | AlwaysTruthy`
       (new addition is ignored since it is a subtype
       of pre-existing element `AlwaysTruthy)

Final union: `Literal[False] | AlwaysTruthy`
```

There are the following set of equivalent type pairs that can currently be constructed under our current model but are not understood to be equivalent by red-knot:

- `AlwaysTruthy | bool ≡ AlwaysTruthy | Literal[False]`
- `AlwaysFalsy | bool ≡ AlwaysFalsy | Literal[True]`
- `~AlwaysTruthy | bool ≡ ~AlwaysTruthy | Literal[True]`
- `~AlwaysFalsy | bool ≡ ~AlwaysFalsy | Literal[False]`

This PR refactors the union builder to ensure that unions are _never_ constructed that contain `bool` as an element; instead, `bool` is always decomposed to `Literal[True, False]`. The same is done for our intersection builder. Doing this makes it much easier to ensure that unions such as the ones above always have the same set of elements no matter which order they are inserted into the union; a lot of complexity is removed from `builder.rs` in this PR. However, it has the (significant) drawback that in various type-relational methods such as `Type::is_equivalent_to()`, `Type::is_subtype_of()` and `Type::is_assignable_to()`, we have to remember to normalize `bool` into `Literal[True, False]` before comparing the type with any other type, since the other type might be a union, and we must ensure that `Literal[True, False]` is understood to be equivalent to `bool`.

Since it would be confusing for users if `bool` was displayed as `Literal[True, False]`, some logic is added to `Type::display` so that `Literal[True, False]` is replaced with `bool` in unions before unions are printed to the terminal in diagnostics.

This PR fixes https://github.com/astral-sh/ty/issues/216, and allows us to promote two flaky property tests to stable. However, it currently shows up as having a 1% performance regression on both red-knot benchmarks on codspeed: https://codspeed.io/astral-sh/ruff/branches/alex%2Ftruthy-unions-5.

## What about literal strings?

As well as the above invariants for unions containing `AlwaysTruthy/AlwaysFalsy` and `bool`, there are also equivalences for `LiteralString` that we do not yet understand. The fact that we do not understand these is at least partly responsible for the flakiness of several other property tests that are not marked as stable:

- `AlwaysTruthy | LiteralString ≡ AlwaysTruthy | Literal[""]`
- `AlwaysFalsy | LiteralString ≡ AlwaysFalsy | LiteralString & ~Literal[""]`
- `~AlwaysTruthy | LiteralString ≡ ~AlwaysTruthy | Literal[""]`
- `~AlwaysFalsy | LiteralString ≡ ~AlwaysFalsy | LiteralString & ~Literal[""]`

I'm not sure if this problem is solvable in the same way without adding a new `Type::TruthyLiteralString` variant. Anyway, this PR does not attempt to solve this problem. Instead, some failing tests are added with a TODO.

## Test Plan

- New mdtests added that fail on `main`
- `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`
- `QUICKCHECK_TESTS=5000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::union_equivalence_not_order_dependent`
- `QUICKCHECK_TESTS=3000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::intersection_equivalence_not_order_dependent`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-25 13:10_

---

_@AlexWaygood reviewed on 2025-01-25 13:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4674 on 2025-01-25 13:13_

hmm, this doesn't yet handle unions inside tuples inside tuples...

---

_Comment by @AlexWaygood on 2025-01-25 16:00_

There's actually two distinct changes here; I'll split this PR into two

---

_Marked ready for review by @AlexWaygood on 2025-01-25 17:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-25 17:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-25 17:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-25 17:21_

---

_@AlexWaygood reviewed on 2025-01-25 17:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:458 on 2025-01-25 17:23_

The order here changes because I'm using `swap_remove` in `Type::display` to ensure that `Literal[True, False]` is displayed as `bool` to users. But maybe it's okay to use `remove()` in `Type::display()` rather than `swap_remove`? `remove()` is `O(n)` rather than `O(1)`, but displaying a type shouldn't really be performance-sensitive: it's usually only done as part of printing a diagnostic to the terminal

---

_@AlexWaygood reviewed on 2025-01-25 17:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:76 on 2025-01-25 17:25_

I changed this because it doesn't look like we've implemented comparisons between unions inside tuples yet, and `bool` is now internally represented as a union, which meant that many `reveal_type` assertions in this test became TODOs rather than `bool`s. Testing whether we knew how to compare a `bool` with a `bool` didn't seem to me to be the point of the test.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-25 17:26_

I had a stack overflow for a looong time here before I realised I had to change this... was tearing my hair out 🙃

---

_@AlexWaygood reviewed on 2025-01-25 17:26_

---

_Comment by @AlexWaygood on 2025-01-26 10:18_

> it has the (significant) drawback that in various type-relational methods such as `Type::is_equivalent_to()`, `Type::is_subtype_of()` and `Type::is_assignable_to()`, we have to remember to normalize `bool` into `Literal[True, False]` before comparing the type with any other type, since the other type might be a union, and we must ensure that `Literal[True, False]` is understood to be equivalent to `bool`.

One solution to this might be to add a `NormalizedType` newtype, which you _have_ to convert a `Type` into before you can use any of these methods. Then we wouldn't have to remember to normalize types inside these methods (or inside the union and intersection builders!) -- we would know that the type was already normalized due to a `NormalizedType` instance being passed rather than a `Type`.

```rs
#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
pub struct NormalizedType<'db>(Type<'db>);

impl<'db> NormalizedType<'db> {
    /// Return true if this type is a [subtype of] type `target`.
    pub(crate) fn is_subtype_of(self, db: &'db dyn Db, target: Self) -> bool {}

    /// Return true if this type is [assignable to] type `target`.
    pub(crate) fn is_assignable_to(self, db: &'db dyn Db, target: Self) -> bool {}

    /// Return true if this type is [equivalent to] type `other`.
    pub(crate) fn is_equivalent_to(self, db: &'db dyn Db, other: Self) -> bool {}

    /// Returns true if this type and `other` are gradual equivalent.
    pub(crate) fn is_gradual_equivalent_to(self, db: &'db dyn Db, other: Self) -> bool {}

    /// Return true if this type and `other` have no common elements.
    pub(crate) fn is_disjoint_from(self, db: &'db dyn Db, other: Self) -> bool {}
}

impl<'db> Deref for NormalizedType<'db> {
    type Target = Type<'db>;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

impl<'db> From<NormalizedType<'db>> for Type<'db> {
    fn from(value: NormalizedType<'db>) -> Self {
        value.0
    }
}
```

This is quite attractive in some ways. It would be a very big change to make, though, and it would make our API more awkward to use in several places. I'm not sure it would be worth it.

---

_@sharkdp reviewed on 2025-01-27 08:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-27 08:07_

Can you say more why this is necessary? And maybe add a comment (unless I'm missing something obvious)?

---

_@AlexWaygood reviewed on 2025-01-27 11:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-27 11:02_

The first step in `Type::is_subtype_of()` is now (with this PR) to normalize `bool` to `Literal[True] | Literal[False]` for both `self` and `target`. So if `self` is `bool`, `self.is_subtype_of(target)` delegates to whether `Literal[True] | Literal[False]` is a subtype of `target`.  That takes us to the `Type::Union` branch: a union type is a subtype of `target` iff all its members are a subtype of `target`. So then we try the first member of the union `Literal[True] | Literal[False]`, and ask the question: "Is `Literal[True]` a subtype of `target`? If `target` is not equivalent to `Literal[True]`, that takes us to this branch, and this branch says "`Literal[True]` is a subtype of `target` iff `bool` is a subtype of `target`". So then we ask the question "Is `bool` a subtype of `target`?", which is the very first question we started off with. And `bool` is normalized to `Literal[True] | Literal[False]`... the cycle repeats indefinitely, resulting in infinite recursion.

---

_@sharkdp reviewed on 2025-01-27 11:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-27 11:21_

Thanks!

I still don't understand why it's correct though? There are types which are supertypes of `bool`, but not of `int`. `bool | None` or `~Literal[2]`, for example. So why is it correct to replace the `bool <: target` check with a `int <: target` check? Because all special cases have been handled above?

---

_@AlexWaygood reviewed on 2025-01-27 11:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-27 11:54_

> I still don't understand why it's correct though?

It might not be -- I _have_ been known to make mistakes 😜

> There are types which are supertypes of `bool`, but not of `int`. `bool | None` or `~Literal[2]`, for example. So why is it correct to replace the `bool <: target` check with a `int <: target` check? Because all special cases have been handled above?

Yeah, I _think_ our set-theoretic types such as unions and intersections should be handled in the branches above? I just added more test cases demonstrating this.

(I _will_ add a comment to the code reflecting this conversation, but first I'm experimenting with alternative ways of structuring the code that might make this clearer in the first place!)

---

_@AlexWaygood reviewed on 2025-01-27 18:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:995 on 2025-01-27 18:25_

(https://github.com/astral-sh/ruff/pull/15773 is the "alternative way of structuring the code", for anybody else reading this thread)

---

_@carljm reviewed on 2025-01-28 00:48_

I think I suggested normalizing `bool` to `Literal[True, False]`, and I appreciate you exploring what that would look like! But having seen what it would look like, it doesn't feel to me like the right direction.

The cases where this simplification applies are very narrow: unions of `bool` (or an enum, in future -- I'm going to call these sealed types) with a larger type that only partially overlaps with `bool` (or the enum). Since `bool` and enums are both very narrow types, with a small/fixed set of singleton inhabitants, there are very few larger types that will partially overlap. Some types (`int` or `object`) will fully overlap (they are super-types), but I think in fact `AlwaysTruthy` and `AlwaysFalsy` are the only partially overlapping types?

So while decomposing is attractive in its conceptual simplicity (and in its code simplicity!), it means lots of operations on bools in unions become more expensive, because we have to check against two types instead of one (and this will be even worse with enums, possibly making the approach not even workable for them at all.) And then in the effort to limit some of that inefficiency (by decomposing only in unions/intersections), we lose a fair bit of the conceptual and code simplicity. So we are kind of letting the tail wag the dog, by paying extra perf cost for all uses of `bool` intersections, in order to support the less-common case where `Always{Truthy,Falsy}` is also in that union.

I think we do want to normalize these cases (by which I mean, where a sealed type partially overlaps with another type in a union) such that we decompose the sealed type and eliminate the overlap (that is, `AlwaysFalse | Literal[True]` instead of `AlwaysFalse | bool`). This seems better than trying to encode the equivalence in `is_equivalent_to`.

But given that the cases where this applies are so narrow, and we already compare every new union element to every existing union element, I think we can do this as (yet another) special case in `UnionBuilder`, and that will perform better than always decomposing.

The rule would be that whenever we see `bool` in a union with a type that it is not disjoint from but also not a subtype of (I phrase it in this general way so that we also handle intersections containing `Always{Truthy,False}` being in a union with `bool`), we replace `bool` in that union with its literal inhabitant that is disjoint from the other type.

WDYT?

---

_Comment by @AlexWaygood on 2025-01-28 11:19_

> WDYT?

I think this could work! I tried something like what you outline above initially, and struggled to get it to work, but I think at that stage I still didn't understand the full extent of the problem here. Revisiting it might yield different results now that I am Older and Wiser.

> I think we do want to normalize these cases (by which I mean, where a sealed type partially overlaps with another type in a union) such that we decompose the sealed type and eliminate the overlap (that is, `AlwaysFalse | Literal[True]` instead of `AlwaysFalse | bool`). This seems better than trying to encode the equivalence in `is_equivalent_to`.

I agree that we need some kind of normalization here rather than attempting some ugly (and expensive) special casing in `is_equivalent_to`. And intuitively, normalizing the union elements so that they do not overlap feels like the better way to go. But in practice, I wonder if normalizing to `AlwaysFalsy | bool` might actually be better. If we normalize to `AlwaysFalsy | Literal[True]`, we won't naturally understand `bool` as being a subtype of this type; we'd still have to decompose `bool` to `Literal[True, False]` in `is_subtype_of` and `is_assignable_to`. If we normalize to `AlwaysFalsy | bool`, however, I think it's possible that no special handling will be necessary in those methods.

---

_Comment by @sharkdp on 2025-01-28 14:39_

I think there might be a more general problem here where we should understand the equivalence `P | Q = (P & ~Q) | Q` and `P | Q = P | (Q & ~P)`. We currently don't.

I understand that there is a desire to always find a canonical representation, but I'm afraid this might be too much to ask for in a set-theoretic type system? Enforcing non-overlapping union elements can be achieved, but there are many ways to do that — the two variants `(P & ~Q) | Q` and `P | (Q & ~P)` are just two extreme cases where the entire overlap is either moved to the right or to the left. And if more than two types are involved, there's a combinatorial explosion of possibilities.

It is obviously desirable to have a "simple" and unique representation when we present types to users, at least in common cases. But as a general property, I'm not sure if that is achievable.

---

_Comment by @dcreager on 2025-01-28 16:20_

> I understand that there is a desire to always find a canonical representation,

We've had some PRs that move us in this direction, but I think this is the key question we need to answer — do we want `Type` to have a normal form?  (presumably DNF given its current shape)

> And if more than two types are involved, there's a combinatorial explosion of possibilities.
> 
> It is obviously desirable to have a "simple" and unique representation when we present types to users, at least in common cases. But as a general property, I'm not sure if that is achievable.

...because if so, you would typically avoid that combinatorial explosion by ensuring that the primitives in the normal form are all disjoint.

In this case, it's `AlwaysTruthy` / `AlwaysFalsy` which most obviously violates that disjointness.  So they could not be primitives (i.e. variants in the `Type` enum), and would either be a property that types can have (implemented as an `is_always_truthy` method), or syntactic sugar for some type that is in normal form.  (e.g. an `always_falsy` constructor that would desugar into something like `Union[False, 0, "", some protocol involving __bool__, ...]`)

---

_Comment by @sharkdp on 2025-01-28 16:53_

> ...because if so, you would typically avoid that combinatorial explosion by ensuring that the primitives in the normal form are all disjoint.
> 
> In this case, it's `AlwaysTruthy` / `AlwaysFalsy` which most obviously violates that disjointness. So they could not be primitives (i.e. variants in the `Type` enum), and would either be a property that types can have (implemented as an `is_always_truthy` method), or syntactic sugar for some type that is in normal form. (e.g. an `always_falsy` constructor that would desugar into something like `Union[False, 0, "", some protocol involving __bool__, ...]`)

I was mostly thinking about non-primitives, such as unrelated classes
```py
class C1: ...
class C2: ...
...
class Cn: ...
```
How would you bring the union `C1 | C2 | … | Cn` into some canonical form (that avoids overlaps)? We could try something like `C1 | C2 & ~C1 | C3 & ~C1 & ~C2 | …`, but that depends on insertion order and doesn't look like it would be very useful to work with(?). A symmetric version would probably involve having O(n²) entries (`C1 & ~C2 & … & ~Cn | ~C1 & C2 & ~C3 & … & ~Cn | … | ~C1 & … & ~Cn-1 & Cn)`).

---

_Comment by @carljm on 2025-01-28 17:17_

Yes, (one) problem with trying to avoid all overlaps is that a typical nominal instance type (perhaps the most common type) is not disjoint from another unrelated one, thanks to subclassing and multiple inheritance. So avoiding all overlaps in unions seems impractical.

I agree that I'm not sure it's feasible in general to have a single normalized representation of every type. (It is important that we maintain DNF in the sense that we always normalize to a union of intersections, never a more complex tree of unions and intersections. But that doesn't necessarily imply a fully normalized form in terms of the possible combinations of elements.)

My suggestion above was much more narrow and targeted: that we'd avoid overlaps _with sealed types_ and prefer decomposing the sealed type instead. I think this is more tractable. But Alex makes a good point that this might be the wrong direction; perhaps we should prefer the overlap and use the full sealed type whenever it is correct to do so.

> I think there might be a more general problem here where we should understand the equivalence `P | Q = (P & ~Q) | Q` and `P | Q = P | (Q & ~P)`. We currently don't.

To me this looks like a case where we'd ideally prefer to achieve this understanding via an eager simplification of `(P & ~Q) | Q` to `P | Q` (which again means we are preferring the overlap, not preferring to avoid the overlap.)

---

_Comment by @carljm on 2025-02-01 01:03_

I just realized from looking at the [overloads spec PR](https://github.com/python/typing/pull/1839) that the pattern you started introducing in this PR as `with_normalized_bools` is something we will eventually need for overload matching, under the general rubric of "type expansion". (See the very end of `overloads.rst` in that PR.)

---

_Comment by @MichaReiser on 2025-04-28 07:09_

Just checking in. Is this something that we still plan on merging/changing in the future?

---

_Comment by @AlexWaygood on 2025-04-28 07:11_

> Just checking in. Is this something that we still plan on merging/changing in the future?

I've deprioritised this issue for now as it's not urgent, but it's still something we'll need to fix at some point, and I still think something like this could be a viable fix. I'd rather not close this just yet.

---
