```yaml
number: 16636
title: "[red-knot] Improve is_disjoint for two intersections"
type: pull_request
state: merged
author: jgeralnik
labels:
  - ty
assignees: []
merged: true
base: main
head: joey/negative_is_disjoint
created_at: 2025-03-11T16:43:03Z
updated_at: 2025-03-14T17:56:32Z
url: https://github.com/astral-sh/ruff/pull/16636
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Improve is_disjoint for two intersections

---

_@jgeralnik_

## Summary

Background - as a follow up to #16611 I noticed that there's a lot of code duplicated between the `is_assignable_to` and `is_subtype_of` functions and considered trying to merge them. 

[A subtype and an assignable type are pretty much the same](https://typing.python.org/en/latest/spec/concepts.html#the-assignable-to-or-consistent-subtyping-relation), except that subtypes are by definition fully static, so I think we can replace the whole of `is_subtype_of` with:

```
if !self.is_fully_static(db) || !target.is_fully_static(db) {
    return false;
}
return self.is_assignable_to(target)
```

if we move all of the logic to is_assignable_to and delete duplicate code. Then we can discuss if it even makes sense to have a separate is_subtype_of function (I think the answer is yes since it's used by a bunch of other places, but we may be able to basically rip out the concept).

Anyways while playing with combining the functions I noticed is that the handling of Intersections in `is_subtype_of` has a special case for two intersections, which I didn't include in the last PR - rather I first handled right hand intersections before left hand, which should properly handle double intersections (hand-wavy explanation I can justify if needed - (A & B & C) is assignable to (A & B) because the left is assignable to both A and B, but none of A, B, or C is assignable to (A & B)).

I took a look at what breaks if I remove the handling for double intersections, and the reason it is needed is because is_disjoint does not properly handle intersections with negative conditions (so instead `is_subtype_of` basically implements the check correctly).

This PR adds support to is_disjoint for properly checking negative branches, which also lets us simplify `is_subtype_of`, bringing it in line with `is_assignable_to`  

## Test Plan

Added a bunch of tests, most of which failed before this fix

---

_Review requested from @carljm by @jgeralnik on 2025-03-11 16:43_

---

_Review requested from @MichaReiser by @jgeralnik on 2025-03-11 16:43_

---

_Review requested from @AlexWaygood by @jgeralnik on 2025-03-11 16:43_

---

_Review requested from @sharkdp by @jgeralnik on 2025-03-11 16:43_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-11 16:44_

---

_Renamed from "Improve is_disjoint for two intersections" to "[red-knot] Improve is_disjoint for two intersections" by @jgeralnik on 2025-03-11 16:48_

---

_Comment by @github-actions[bot] on 2025-03-11 16:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @jgeralnik on 2025-03-11 17:04_

~~This doesn't work with gradual types :(~~

~~This assert will fail with the current implementation:~~

~~`static_assert(not is_disjoint_from(Any, Not[X]))`~~

~~Will think it over, in the meanwhile this PR can be marked as a draft~~

(Never mind, it works perfectly fine - I made a mistake running the test)

---

_Converted to draft by @jgeralnik on 2025-03-11 17:04_

---

_Marked ready for review by @jgeralnik on 2025-03-11 17:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:134 on 2025-03-11 20:36_

This is only true for fully-static types, since if we have dynamic types involved, `Any` on each side can represent an arbitrarily different type. In the simplest case, `Any` is not disjoint from `Not[Any]`: `Not[Any]` is indistinguishable from `Any`, and clearly `Any` is not disjoint from `Any`. Maybe we should add some more tests for this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:984 on 2025-03-11 20:39_

I think using `is_assignable_to` here is not right, but using `is_subtype_of` might be right? (See my comment above that `Any` and `Not[Any]` are not disjoint.)

---

_@carljm reviewed on 2025-03-11 20:47_

Thank you!

So something I failed to mention (or remember to do myself) on your previous PR: we have a set of quickcheck property tests, which check a bunch of invariants that should apply to type relations, using randomly-generated types. These don't run in CI (because they are inherently slow and non-deterministic), but they can be very helpful in catching gaps in our understanding of type relations. To run them, I use `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`. (You can adjust the `QUICKCHECK_TESTS` number to tradeoff test running time with confidence that any failure cases should be caught.)

Your previous PR actually broke a couple of these (`all_type_pairs_are_assignable_to_their_union` and `subtype_of_implies_assignable_to`), but this PR seems to fix those! This PR, however, does break `disjoint_from_is_irreflexive` and `disjoint_from_is_symmetric`:

```
---- types::property_tests::stable::disjoint_from_is_irreflexive stdout ----

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [Union([BuiltinClassLiteral("bool"), KnownClassInstance(TypeAliasType)])] })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::disjoint_from_is_symmetric stdout ----

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [KnownClassInstance(NoDefaultType)] }, Intersection { pos: [LiteralString], neg: [KnownClassInstance(TypeAliasType)] })
```

Also, we have some parallel work going on here accidentally, another contributor just submitted https://github.com/astral-sh/ruff/pull/16641, which also fixes the two property tests currently failing on main, and stabilizes a couple other property tests that are currently flaky. It looks like there is some overlap between that PR and this one, but that one does some additional stuff around understanding literal integer and string types and their relationship to `AlwaysTruthy` and `AlwaysFalsy`.

Ultimately I'll merge whichever of these reaches "all green" (including all stable property tests) first (and passes review otherwise), and the other one can rebase on top and add whatever is left.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:608 on 2025-03-11 20:49_

Pretty cool that this whole case can be eliminated by improving `is_disjoint_from`! Really nice find.

---

_@carljm reviewed on 2025-03-11 20:53_

> I think we can replace the whole of `is_subtype_of`

Yes, I think that should be true. The main question would be whether this has any negative impact on performance, since `is_subtype_of` is used heavily in union type simplification. (It is valid to elide type A from `A | B` if `A` is a subtype of `B`, but not if `A` is just assignable to `B`; the latter would wrongly simplify every `Any | T` to just `Any` (or just `T`! It would be arbitrary which one; the non-transitivity of `is_assignable_to` with dynamic types is a problem here.) I suspect maybe there won't be any performance issue, though, it's certainly worth trying.

I don't think we can (or should) eliminate the _concept_ of `is_subtype_of`; I think it is necessary to have the concept (and the method). But totally open to simplifying the _implementation_ if we can do that without behavior or performance regression.

---

_Review comment by @jgeralnik on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:134 on 2025-03-11 21:17_

Yeah I suspected in the comment I left above that Any was broken but then I couldn't construct a case to assert it. Found one now that I'll add, and replacing is_assignable_to with is_subtype_of fixes it

---

_@jgeralnik reviewed on 2025-03-11 21:17_

---

_@jgeralnik reviewed on 2025-03-11 22:41_

---

_Review comment by @jgeralnik on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:134 on 2025-03-11 22:41_

is_subtype_of is also not a clean fix - regardless of which direction I choose one of these tests fails:

```
static_assert(not is_disjoint_from(Intersection[Any, X], Intersection[Any, Not[Y]]))
static_assert(not is_disjoint_from(Intersection[Any, Not[Y]], Intersection[Any, X]))

static_assert(is_disjoint_from(Intersection[int, Any], Not[int]))
static_assert(is_disjoint_from(Not[int], Intersection[int, Any]))
```

Need to think about this some more tomorrow

---

_Comment by @jgeralnik on 2025-03-11 22:48_

Need to think this over a bit more, will keep an eye on the other PR and just add my nasty test cases to it if it succeeds before me.

---

_Comment by @jgeralnik on 2025-03-12 10:06_

I came up with two solutions for this, both of which work and pass all quickcheck tests, but I'm not necessarily satisified with either. Both cases require special handling of two intersections in `is_disjoint`:

Option 1:
```
            (Type::Intersection(self_intersection), Type::Intersection(other_intersection)) => {
                self_intersection.
                self_intersection
                    .positive(db)
                    .iter()
                    .any(|p| p.is_disjoint_from(db, other))
                    || other_intersection
                        .positive(db)
                        .iter()
                        .any(|p: &Type<'_>| p.is_disjoint_from(db, self))
        }
```
I'm pretty sure we can't have disjoint intersections with only negative elements - a negative element needs a positive element on the other side in order to become disjoint. This does rely on the builder simplifying things - obviously `Not[Not]`s, but also replacing `Not[Any]` with `Any`. This matches the logic that happens in the builder today, where new elements are only compared (with is_disjoint) to other positive elements to check if the resulting intersection is empty.

The other option is to go all in on relying on the builder - to test if two intersections are disjoint we can just combine them into a single intersection and see if the result is never:

```
            (Type::Intersection(_), Type::Intersection(_)) => {
                IntersectionBuilder::new(db)
                    .add_positive(self)
                    .add_positive(other)
                    .build()
                    .is_never()
            }
```

This feels more clever, but is much less efficient - IntersectionBuilder will add items one by one and check them against all current items so is actually O(n**2), and the fact that `IntersectionBuilder` and `is_disjoint` both call into each other makes me squeamish even if it is probably ok since is_disjoint is always called there with a single element.

The first options is actually also O(n**2), but probably we won't have ever intersections with more than a couple dozen elements?

I feel like option 1 is the better choice, will update code shortly

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:156 on 2025-03-12 11:23_

Is this just a leftover comment that should be removed? I'm not seeing any failing quickcheck tests on the current version of the PR, and when I add this example as an mdtest, it passes (in both permutations). (It looks like the tests immediately above here are the same scenario, just using `int` in place of `abc.ABC`.)

```suggestion
```

---

_@carljm approved on 2025-03-12 11:40_

Great work! I'm seeing all stable quickcheck tests passing, even at a million runs.

This PR is the best kind of PR: net negative non-test LOC and improves correctness.

---

_@carljm reviewed on 2025-03-12 11:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:156 on 2025-03-12 11:41_

Going to go ahead and remove this comment, then merge the PR. Let me know if I've missed something that was still TODO represented by this comment!

---

_@jgeralnik reviewed on 2025-03-12 12:04_

---

_Review comment by @jgeralnik on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:156 on 2025-03-12 12:04_

Oh yeah, whoops. Was left over from debugging of quickcheck failures.

---

_Merged by @carljm on 2025-03-12 12:13_

---

_Closed by @carljm on 2025-03-12 12:13_

---

_@AlexWaygood reviewed on 2025-03-14 17:56_

This is very elegantly done. Nice work!!

---
