```yaml
number: 13775
title: "[red knot] add `Type::is_disjoint_from` and intersection simplifications"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dnf-type-simplifications
created_at: 2024-10-16T12:12:30Z
updated_at: 2024-10-18T21:43:45Z
url: https://github.com/astral-sh/ruff/pull/13775
synced_at: 2026-01-10T20:59:37Z
```

# [red knot] add `Type::is_disjoint_from` and intersection simplifications

---

_Pull request opened by @sharkdp on 2024-10-16 12:12_

## Summary

- Add `Type::is_disjoint_from` as a way to test whether two types overlap
- Add a first set of simplification rules for intersection types
  - `S & T = S` for `S <: T`
  - `S & ~T = Never` for `S <: T`
  - `~S & ~T = ~T` for `S <: T`
  - `A & ~B = A` for `A` disjoint from `B`
  - `A & B = Never` for `A` disjoint from `B`
  - `bool & ~Literal[bool] = Literal[!bool]`

resolves one item in astral-sh/ty#245

## Open questions:

- Can we somehow leverage the (anti) symmetry between `positive` and `negative` contributions? I could imagine that there would be a way if we had `Type::Not(type)`/`Type::Negative(type)`, but with the `positive`/`negative` architecture, I'm not sure. Note that there is a certain duplication in the `add_positive`/`add_negative` functions (e.g. `S & ~T = Never` is implemented twice), but other rules are actually not perfectly symmetric: `S & T = S` vs `~S & ~T = ~T`.
- I'm not particularly proud of the way `add_positive`/`add_negative` turned out. They are long imperative-style functions with some mutability mixed in (`to_remove`). I'm happy to look into ways to improve this code *if we decide to go with this approach* of implementing a set of ad-hoc rules for simplification.
- ~~Is it useful to perform simplifications eagerly in `add_positive`/`add_negative`? (@carljm)~~ This is what I did for now.

## Test Plan

- Unit tests for `Type::is_disjoint_from`
- Observe changes in Markdown-based tests
- Unit tests for `IntersectionBuilder::build()`

---

_Label `red-knot` added by @sharkdp on 2024-10-16 12:12_

---

_@sharkdp reviewed on 2024-10-16 12:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:479 on 2024-10-16 12:18_

There are 18 enum variants in `Type`, so 18 × 18 = 324 cases to cover. `is_disjoint_from` is symmetric, so in principle it's only one triangular part of the matrix, but that still leaves 171 cases. Not sure this is the best way to structure them...

I also tried nested `match` statements, but that makes the symmetry less clear.

---

_Comment by @github-actions[bot] on 2024-10-16 12:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-10-17 11:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-10-17 11:28_

I'm playing things save here, since
```rs
!class_type.is_known(db, KnownClass::Int)
```
would potentially be wrong here, I assume? There might be a subclass of `int`, in which case instances of that subclass would potentially overlap with an `IntLiteral`.

And using something like
```rs
ty@Type::Instance(..) => !ty.is_subtype_of(KnownClass::Bool.to_instance(db))
```
is dangerous, since `is_subtype_of` can return false-negative answers (return `false` for types that are not actually subtypes of each other).

---

_@sharkdp reviewed on 2024-10-17 11:29_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:548 on 2024-10-17 11:29_

Is this save? My thinking was that `NoneType` can't be subclassed.

---

_@sharkdp reviewed on 2024-10-17 11:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:572 on 2024-10-17 11:30_

Same question here. `bool` is also final.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:548 on 2024-10-17 11:35_

`NoneType` is however a subclass of `object`, so `Type::Instance(builtins.object)` is not disjoint from `Type::None`

```pycon
>>> type(None).__mro__
(<class 'NoneType'>, <class 'object'>)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:572 on 2024-10-17 11:36_

Same issue here. `Literal[True]` is a subtype of `bool`, but `bool` is a subtype of `object` (all classes in Python are subclasses of `object`, either explicitly or implicitly)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-10-17 11:37_

I don't think subclasses of `int` are an issue here. An instance of an `int` subclass cannot be considered an `IntLiteral`; only an object `o` where `o.__class__ == int` can be considered an `IntLiteral`

---

_@sharkdp reviewed on 2024-10-17 11:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:352 on 2024-10-17 11:38_

I didn't bother trying to integrate this with the new simplification logic, as I saw that there is a plan to remove `Type::Unbound` (#13671)

---

_@sharkdp reviewed on 2024-10-17 11:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-10-17 11:41_

This was certainly more efficient before. There are certainly ways to improve performance of `add_positive`/`add_negative`. Is it something I should look into now, or is it okay to postpone that to the point were the runtime of intersection-type-simplification becomes a problem (if ever).

---

_@AlexWaygood reviewed on 2024-10-17 11:42_

---

_@AlexWaygood reviewed on 2024-10-17 11:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-10-17 11:44_

This is spec'd here: https://typing.readthedocs.io/en/latest/spec/literal.html#equivalence-of-two-literals

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-10-17 12:37_

Thanks!

---

_@sharkdp reviewed on 2024-10-17 12:37_

---

_Marked ready for review by @sharkdp on 2024-10-17 12:40_

---

_Review requested from @carljm by @sharkdp on 2024-10-17 12:40_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-17 12:40_

---

_@AlexWaygood reviewed on 2024-10-17 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:551 on 2024-10-17 12:42_

Or maybe:

```suggestion
                Type::Instance(class_type) => !matches!(
                    class_type.known(db),
                    Some(KnownClass::NoneType | KnownClass::Object)
                )
```

---

_@AlexWaygood reviewed on 2024-10-17 12:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:552 on 2024-10-17 12:59_

It seems like this is actually fewer lines (and has exhaustiveness verified at compile time, which is nice) if you do it without the inner `match`. It _does_ feel less readable to do it that way, though, so I'm not sure if it's worth it:

```suggestion
            (Type::None, Type::None) => false,
            (Type::None, Type::Instance(class)) | (Type::Instance(class), Type::None) => !matches!(
                class.known(db),
                Some(KnownClass::NoneType | KnownClass::Object)
            ),
            (
                Type::None,
                Type::IntLiteral(_)
                | Type::BooleanLiteral(_)
                | Type::StringLiteral(..)
                | Type::LiteralString
                | Type::BytesLiteral(..),
            )
            | (
                Type::IntLiteral(_)
                | Type::BooleanLiteral(_)
                | Type::StringLiteral(..)
                | Type::LiteralString
                | Type::BytesLiteral(..),
                Type::None,
            ) => true,
```

---

_@AlexWaygood reviewed on 2024-10-17 13:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1834 on 2024-10-17 13:02_

nit: I'd find this slightly more readable

```suggestion
        Intersection {
            pos: Vec<Ty>,
            neg: Vec<Ty>,
        },
```

---

_@AlexWaygood reviewed on 2024-10-17 13:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-10-17 13:04_

I think the performance of intersections will likely be quite important in the long run (we'll probably create _lots_ of intersections once our model is more fleshed out). That doesn't necessarily mean that we need to micro-optimise the initial implementation, I don't think (we can always iterate to improve performance), but it definitely means we should be mindful of performance at every stage.

---

_@sharkdp reviewed on 2024-10-17 13:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-10-17 13:15_

> we'll probably create lots of intersections once our model is more fleshed out

but will we also create *large* intersections? Since we have this eager approach now where we simplify immediately, what would be an example for an intersection type that would grow large (>= 3 elements)?

---

_@AlexWaygood reviewed on 2024-10-17 13:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-10-17 13:17_

Good question! Not really sure how this is going to play out, in all honesty. Possibly @carljm has a better answer for you here :-)

---

_@sharkdp reviewed on 2024-10-17 13:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:552 on 2024-10-17 13:39_

I think I prefer yours. My eyes hurt a bit, but it's good to get rid of the `unreachable!(…)`s.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:233 on 2024-10-17 20:57_

Could this use find to exit early?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:255 on 2024-10-17 20:58_

Nit: add break?

---

_@MichaReiser reviewed on 2024-10-17 20:59_

---

_Comment by @carljm on 2024-10-17 22:28_

> Can we somehow leverage the (anti) symmetry between positive and negative contributions? I could imagine that there would be a way if we had Type::Not(type)/Type::Negative(type), but with the positive/negative architecture, I'm not sure.

FWIW, I'm quite open to the idea that we discard the positive/negative approach in favor of a `Type::Not`, if it makes intersection simplification easier.

The downside is we then have to add handling for `Type::Not` throughout the type system, but I suspect that may actually not be too difficult, since in many places we can just treat it as `object`, which is less precise but never unsound.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:9 on 2024-10-17 22:31_

Love seeing these TODOs go away :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:479 on 2024-10-17 22:34_

FWIW I think this flat approach is better than nested, and matches what we do elsewhere for binary operations on types. It is a large space to cover (and will only get larger with time), but it helps a lot that we can usually have blanket cases for several types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:524 on 2024-10-17 23:02_

I think we can't be sure of this, at least not considering types we will support in future, without doing more checks.

* `other` might be the homogenous arbitrary-length tuple type `tuple[T, ...]` (which we don't have support for yet); if all of our element types are subtypes of T, this is not disjoint
* `other` might be a user subtype of `tuple`, which, if generic over the same or compatible `*Ts`, would overlap with `Type::Tuple`

Both of these cases involve types we can't yet represent, so in a sense the `true` here is correct in terms of the types we are currently able to represent. But given that `other` is wildcard-matched here and we know such types exist in principle and we will support them in future, I think we should replace this `true` with `false`, plus a comment outlining the reasons why, and a TODO to actually check these cases, once we have the appropriate types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:541 on 2024-10-17 23:04_

I'm curious why you use `_` for `BooleanLiteral` but `..` for `StringLiteral` and `BytesLiteral`, since all three are length-one tuple variants. Not that it matters, just curious :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:543 on 2024-10-17 23:12_

For any two types which are both one of `None`, `BooleanLiteral`, `IntLiteral`, `StringLiteral`, and `BytesLiteral`, either the two types are identical or they are disjoint. (We can rely on this because Salsa already gives us type interning based on equality for `StringLiteralType` and `BytesLiteralType`.)

I think you could have a single case for this, and it would mostly (with the exception of `LiteralString`) subsume this case, and I think some others below as well?

`LiteralString` should be manageable with a single case checking if the other type is `object` or `str`, otherwise it is disjoint.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:479 on 2024-10-17 23:13_

Do you think it's worth an `if self == other { return false }` shortcut here? That might simplify some cases below.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:531 on 2024-10-17 23:18_

Since `None` is disjoint with every non-union type that isn't `None` or `object`, and we already handled unions above, wouldn't this give us more precise results (that is, more correct `true` results) if we matched with any `other` type (not only instance types), and then returned `false` only if `other` is an `Instance` of `object` or `NoneType`, and otherwise returned `true`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:566 on 2024-10-17 23:25_

I think these two cases would both be subsumed by my suggestion above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:571 on 2024-10-17 23:34_

Similar comment to the one above about `None`: since a `BooleanLiteral` is disjoint from every non-union type that isn't `bool` or `object`, can we make this case more general so that it also returns `true` for all non Instance types?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:581 on 2024-10-17 23:35_

These would also be subsumed by above comments, I think?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:545 on 2024-10-17 23:35_

Similar comment as above for `None` and `BooleanLiteral`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:554 on 2024-10-17 23:36_

Similar as for `None`, `BooleanLiteral`, `IntLiteral`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:592 on 2024-10-17 23:37_

Would be subsumed by the general case for literal v literal

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:588 on 2024-10-17 23:37_

Would be subsumed by the general case for literal v literal

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:549 on 2024-10-17 23:38_

I think this would be subsumed by the more general case for `LiteralString` I suggested above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:601 on 2024-10-17 23:39_

This case should be `true`, I think? The `LiteralString` type does not include any bytes objects. Would also be subsumed by the general case for `LiteralString` suggested above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:557 on 2024-10-17 23:40_

Would be subsumed by a general check for identity up top

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:562 on 2024-10-17 23:41_

Same comment as for various types above that this could match vs any type, not just `Type::Instance`; `LiteralString` is disjoint from any non-union type that is not `StringLiteral`, `str`, or `object`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:617 on 2024-10-17 23:43_

I think `false` is the correct answer here in general, barring at least one of the types being `final`, since Python allows multiple inheritance. (Could update the TODO to be more specific that `final` support is all there is left to do?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:228 on 2024-10-17 23:52_

I suspect intersections will not typically grow too large, because most intersections will come from type narrowing, and it would require a lot of nested conditionals to grow a large intersection this way. I think large unions (particularly of literals) will be more of a problem.

That said, I'm quite sure that at some point down the line, someone will present us with some pathological generated-code monstrosity that manages to create a very large intersection, so I suspect we will have to care to some degree :)

I guess I don't have a super clear answer here; I think we should take obvious opportunities to optimize if they don't cost extra hours/days, and then wait and see what we find with real code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:759 on 2024-10-18 00:15_

very minor nit: I prefer to use `#[test_case]` for this kind of thing so they fail or pass independently

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:220 on 2024-10-18 00:25_

I think this TODO comment is addressed by the new code below and can now be removed. We no longer have the old code that cancels positive vs negative based on `Type` equality (which was wrong for dynamic types), it's now subsumed by the pos v neg subtype check, which correctly handles dynamic types, since they don't participate in the subtype relation.

It would be good to add a test verifying that `Any & ~Any` does not simplify to `Never` (and same for `Unknown` and `Todo`).

I think, however, that it should simplify to just `Any`. Adding `Any`, `Unknown`, or `Todo` to the negative side of an intersection should always just add them to the positive side of the intersection instead. The two are equivalent: "must not be in some unknown set of values" is equivalent to "must be in some unknown set of values"; the one "unknown set of values" is just the complement of the other. Since it's an unknown set anyway, we are free to replace it with its complement. Since this will be an easy change to make, and we want to add a related test anyway, I think we might as well add it to this PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 00:41_

With the elimination of this code, I don't think we anymore depend on set operations (or `Type` equality at all) for `positive` or `negative`. Should we just make them vectors instead (like we already did for union elements), since that's lighter weight? In general we should be using semantic `Type` operators (`is_subtype_of`, `is_disjoint_from`) rather than simple `Type` object equality to make most decisions about intersection simplification anyway.

It might require one additional check to avoid adding exactly the same type to the positive or negative side that is already there, but that's a one-line check, we don't need all the `IndexSet` overhead for that.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:242 on 2024-10-18 00:45_

I hadn't really processed from reading the tests below that in some of these cases the pairwise simplification also means we can eliminate anything else that's in the intersection; would it make sense to add cases with "third types" in the below tests to show explicitly which cases do or don't allow that simplification?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:236 on 2024-10-18 00:46_

Since either one is easy to do, which is cheaper, replacing `self` entirely or clearing `self.positive` and `self.negative`? Does the former have to allocate a new pair of `IndexSet` on the heap, and the latter not? (Maybe creating an empty `IndexSet` doesn't allocate anything on the heap until you add the first element, in which case we'd really only allocate once here.)

Same question applies to wherever we replace `*self` in this test.

(I don't think it's worth putting time into benchmarking or researching this at this point, just asking the question in case those of you with more Rust experience have an immediate intuition about it.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:255 on 2024-10-18 00:52_

It's possible that the new positive is a subtype of more than one existing element in the intersection, which themselves have no subtype relationship to each other. In this case we can remove more than one.

However, I don't think we can exercise that case yet, because I think it only occurs with multiple inheritance (or even more advanced types like Protocol), which would require proper `is_subtype_of` support for `Type::Instance`, which in turn would require MRO. Maybe a TODO to support this scenario?

Until we do, breaking here makes sense.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:276 on 2024-10-18 00:59_

Similar comment here: it is possible for two types to already be in `self.negative`, and the new positive type to be disjoint from both of them. In this case we should remove all the existing negative types that are disjoint from the new positive type.

This case I think we can easily test with existing types: if `self.negative` already has two different `IntLiteral` in it and we add a different `IntLiteral` to `positive`, for example.

One implementation thought: presuming this suggests using a vector for `to_remove`, we could consider `smallvec` backed by a length-1 array to avoid heap allocation in the typical 0-or-1 cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:289 on 2024-10-18 00:59_

Similarly I think this comment can be removed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:315 on 2024-10-18 01:03_

Same comment as in other `to_remove` cases; we can have two different existing negative types, and add a new negative type that is a supertype of both.

This also should be testable now: we have `~IntLiteral[1] & ~IntLiteral[2]` and we add `~int`

---

_@carljm requested changes on 2024-10-18 01:12_

This is great! I suspect that this approach will carry us a long way; I'm not feeling any particular need to explore more radical approaches. (I'm not 100% sure, but I suspect that the lazy-BDD approach I linked you to may just not be workable anyway for the complexity of Python's type system; particularly inheritance and multiple inheritance.)

---

_@sharkdp reviewed on 2024-10-18 08:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:541 on 2024-10-18 08:23_

Just an oversight. I like to use `..` for matching on AST/Type enums (even if they have just one element *now*) because they tend to appear in *lots* of `match` statements. If I can be certain that changing the structure of a particular item will not require someone to revisit this pattern, I can make future refactorings easier by using `..` instead of `_`, `_, _`, ….

---

_@sharkdp reviewed on 2024-10-18 08:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:236 on 2024-10-18 08:37_

> Since either one is easy to do, which is cheaper, replacing `self` entirely or clearing `self.positive` and `self.negative`?

In an earlier version on this branch, I used `self.positive.clear(); self.negative.clear();` everywhere, but then I changed it to `*self = Self::new()`. Not because it's shorter, but because it seems safer to me. If someone modifies this `struct` to hold additional members, or wants to enforce additional invariants, they don't have to make sure to reset state properly in N different places.

> Does the former have to allocate a new pair of `IndexSet` on the heap, and the latter not? (Maybe creating an empty `IndexSet` doesn't allocate anything on the heap until you add the first element, in which case we'd really only allocate once here.)

For what it's worth, [`OrderMap::new()`](https://docs.rs/ordermap/latest/ordermap/map/struct.OrderMap.html#method.new) does not allocate. It's still true that using `.clear()` twice might be more efficient here because we could re-use the existing allocations when inserting new elements (later).

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:233 on 2024-10-18 09:26_

That's much better, yes.

---

_@sharkdp reviewed on 2024-10-18 09:26_

---

_@sharkdp reviewed on 2024-10-18 12:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:255 on 2024-10-18 12:47_

To be honest, I'd rather not `break` here. I feel it adds a lot of mental overhead and has questionable benefit.

If we add a `break`, we need to make sure that other — potentially more powerful — simplifications in the same loop (right now, only the *"A & B = Never, if A and B are disjoint"* rule below) can not trigger *in addition* to this one. Right now, the only counter-example I can think of would be something like `(int & ~Literal[1]) & Never`. When we add `Never` as a third element to the intersection, this `new_positive <: existing_positive` rule triggers first, because `Never <: int`, and would lead to a removal of `int`. However, if we keep going, we would find out that `Never` and `int` are also disjoint! This would not just result in a `break`, but in a `return`, because we can immediately conclude that the overall result will be `Never`. We would not just skip this loop, but also the "negative" loop below.

I tried to write a test-case to show that `break`ing here is wrong, and eventually gave up. The example from above works in both cases (with and without `break`) because there are other simplifications in the `negative` branch below. But I'm not sure it invalidates my argument from above. I find it much easier to reason about this code (in the future, if we add more simplifications) if we don't add additional `break`s.

---

_@sharkdp reviewed on 2024-10-18 13:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:524 on 2024-10-18 13:00_

Thank you. I hope it's okay if I use your explanation almost verbatim as a comment.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:255 on 2024-10-18 13:04_

I think it would be good in that case to add a comment why we aren't breaking early. I could see myself come by this code and add the break and then test if there are any failing tests.

---

_@MichaReiser reviewed on 2024-10-18 13:04_

---

_@sharkdp reviewed on 2024-10-18 13:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:543 on 2024-10-18 13:12_

That is a great suggestion — thank you!

---

_@sharkdp reviewed on 2024-10-18 13:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:479 on 2024-10-18 13:26_

It might make some cases unnecessary, but then we would have to add `unreachable!(…)`, right?

Note: the shortcut leads to the wrong result if `self = other = Type::Never`, so it would need an additional condition to handle `Never`.

---

_@sharkdp reviewed on 2024-10-18 13:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:531 on 2024-10-18 13:27_

Changed. And added tests for every possible `None`-vs-X combination, since it seems particularly important.

---

_@sharkdp reviewed on 2024-10-18 13:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:543 on 2024-10-18 13:43_

> `LiteralString` should be manageable with a single case checking if the other type is `object` or `str`, otherwise it is disjoint.

And we need to handle `!is_disjoint_from(LiteralString, StringLiteral(…))`.

---

_@sharkdp reviewed on 2024-10-18 13:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:549 on 2024-10-18 13:44_

No, I think this is required.

---

_@sharkdp reviewed on 2024-10-18 13:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:601 on 2024-10-18 13:45_

Yeah, I don't know what I thought there. Fixed & added test.

---

_@sharkdp reviewed on 2024-10-18 15:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 15:30_

I did that change locally. It works fine, but I'm not sure I like it. 

> It might require one additional check to avoid adding exactly the same type to the positive or negative side that is already there, but that's a one-line check, we don't need all the `IndexSet` overhead for that.

But that's exactly what a set is designed for, right? We don't neet `OrderSet` — we could also use `HashSet` (because I assume we do not need to maintain insertion order for intersections, just for unions?). But turning every insertion/pop into an O(n) operation feels wrong. And we would probably want to wrap the `Vec` in a new datastructure for that, to make sure noone calls `.push` directly.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:242 on 2024-10-18 15:31_

Yes. This particular task is still open.

---

_@sharkdp reviewed on 2024-10-18 15:31_

---

_@sharkdp reviewed on 2024-10-18 15:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:276 on 2024-10-18 15:32_

This makes sense. This task is also still open.

> One implementation thought: presuming this suggests using a vector for `to_remove`, we could consider `smallvec` backed by a length-1 array to avoid heap allocation in the typical 0-or-1 cases.

:+1: 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:490 on 2024-10-18 16:04_

It just occurred to me now on second read-through that this case is incorrect if it comes before the union case, because the union of two `Type::Function` is not disjoint from one of those same `Type::Function`.

I think this case is really the same as the `None` and `Literal` types case below, and could probably just be merged with it?

Would be good to add a test case that would have caught this (to create a union of function or class types, define them in if/else branches.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:524 on 2024-10-18 16:37_

Sure, no problem, though I just realized my comment is imprecise:

> if all of our element types are subtypes of T, this is not disjoint

should be "non-disjoint with T" rather than "subtypes of T"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 16:45_

I think it is already "wrapped" in the `IntersectionType` / `IntersectionBuilder`, so I'm not sure we'd need to wrap it again; the intersection builder code is already quite subtle, and a direct push to an internal vector that doesn't go through `add_negative` / `add_positive` would I think stand out as a bug more than many other bugs we might introduce.

> turning every insertion/pop into an O(n) operation feels wrong

For insertions, they already are; I assume we could put this check inside our existing loop over `self.positive` (in `add_positive`) and over `self.negative` (in `add_negative`). But you're right this might make the removals we have to do in the builder sometimes a bit more expensive? Though I expect that for small intersections, hashing overhead might cost more.

I'm really just dumping some thoughts here for future. If you feel the change doesn't work out well, I'm happy to leave it as-is for now. This tradeoff between the hashing/allocation overhead of a hashset and the extra cost of pops is something that should really be decided by empirical performance data at some point when we do more focused optimization of intersection builder, not by guesses.

> because I assume we do not need to maintain insertion order for intersections, just for unions

Good point! At least that is true until/unless we have a PEP for intersection types that gives users a way to spell them directly in annotations; then we might want to preserve their spelling when we can.


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:531 on 2024-10-18 16:48_

This comment (and several like it below) are resolved, but I don't see the corresponding change in the code? Maybe just not committed/pushed yet?

---

_@carljm reviewed on 2024-10-18 16:49_

Looks like this is still in-process, so didn't do a full re-review of the code, just a few responses to inline comments.

---

_@sharkdp reviewed on 2024-10-18 17:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 17:35_

> I think it is already "wrapped" in the `IntersectionType` / `IntersectionBuilder`, so I'm not sure we'd need to wrap it again

There are several direct calls to `OrderSet::insert` in **`Inner`**`IntersectionBuilder`, so if we replace that with `Vec`, we would need to wrap it again, or make sure we only call a helper function (`self.push_positive(type)`) instead of calling `self.positive.push(…)` directly.

> > turning every insertion/pop into an O(n) operation feels wrong
> 
> For insertions, they already are

(Oops. I meant to say "every insertion/push" there, not "pop")

Why? [`OrderSet::insert`](https://docs.rs/ordermap/latest/ordermap/set/struct.OrderSet.html#method.insert) is O(1), amortized.

> This tradeoff between the hashing/allocation overhead of a hashset and the extra cost of pops is something that should really be decided by empirical performance data

Yes, please! :smile: I think O(n) analysis might be completely misguided here. Even if someone has 10 nested `if` statement and manages to construct an intersection type that really has 10 elements inside (Maybe `int & ~Literal[1] & ~Literal[2] & … & ~Literal[9]`), an O(n²) operation wouldn't be the end of the world. My feeling is that we don't burn any bridges by not doing that directly, but I'd be glad to write some benchmarks and include them in this PR.

---

_@carljm reviewed on 2024-10-18 17:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 17:38_

I wouldn't spend time on benchmarks for this right now. Let's leave it as is, and revisit when our optimization work can be directed and motivated by profiles from real-world large codebases.

---

_@carljm reviewed on 2024-10-18 17:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:531 on 2024-10-18 17:40_

Never mind, I see now (after you pointed it out to me) that you instead handled this via new separate cases, for flatter matching. Makes sense, sorry for the noise!

---

_@sharkdp reviewed on 2024-10-18 18:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:225 on 2024-10-18 18:03_

ok

---

_@sharkdp reviewed on 2024-10-18 19:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:242 on 2024-10-18 19:50_

I added this now for all simplifications.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:276 on 2024-10-18 19:50_

I added this now.

---

_@sharkdp reviewed on 2024-10-18 19:51_

---

_@sharkdp reviewed on 2024-10-18 19:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:315 on 2024-10-18 19:51_

Added this as a test.

---

_@sharkdp reviewed on 2024-10-18 20:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:490 on 2024-10-18 20:24_

> It just occurred to me now on second read-through that this case is incorrect if it comes before the union case, because the union of two `Type::Function` is not disjoint from one of those same `Type::Function`.

Ah, you're right.

> I think this case is really the same as the `None` and `Literal` types case below, and could probably just be merged with it?

:+1: 

> Would be good to add a test case that would have caught this (to create a union of function or class types, define them in if/else branches.)

Yes. I added a test that failed with the previous version.

---

_@sharkdp reviewed on 2024-10-18 20:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:255 on 2024-10-18 20:28_

I first added the comment, but now removed it again. We now support removing multiple elements, so we can't break.

---

_@sharkdp reviewed on 2024-10-18 20:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:261 on 2024-10-18 20:33_

I saw that there was a considerate effort to optimize similar code for unions. I chose to do something much simpler for intersections here, since — for now — we don't care about the order of elements in the intersection. So we can use `swap_remove_index` here which is O(1). We traverse `to_remove` backwards to avoid any index shifts. That is okay, because `to_remove` is sorted.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:537 on 2024-10-18 20:59_

Because `bool` is a subtype of `int` in Python (sadly), we must also include `KnownClass::Int` here.

There was already a test for this case, but it asserted wrongly that `int` and a `BooleanLiteral` were disjoint; I switched it to assert the opposite.

There was also an `IntersectionBuilder` test relying on this assumption of bool/int disjointness; I fixed it by using a `StringLiteral` instead of a `BooleanLiteral`.

---

_@carljm reviewed on 2024-10-18 21:26_

---

_@carljm approved on 2024-10-18 21:30_

This is excellent work, thank you!!

---

_Merged by @carljm on 2024-10-18 21:34_

---

_Closed by @carljm on 2024-10-18 21:34_

---

_Branch deleted on 2024-10-18 21:34_

---
