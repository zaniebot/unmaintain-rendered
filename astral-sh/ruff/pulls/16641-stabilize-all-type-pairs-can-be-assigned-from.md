```yaml
number: 16641
title: "Stabilize `all_type_pairs_can_be_assigned_from_their_intersection`"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
base: main
head: 14899-gradual-type-assignability
created_at: 2025-03-11T18:16:50Z
updated_at: 2025-03-24T14:32:23Z
url: https://github.com/astral-sh/ruff/pull/16641
synced_at: 2026-01-10T19:40:36Z
```

# Stabilize `all_type_pairs_can_be_assigned_from_their_intersection`

---

_Pull request opened by @mtshiba on 2025-03-11 18:16_

## Summary

This PR further fixes the problem in astral-sh/ruff#14899.

It seems that the implementation of `is_assignable_to` for intersection was already done in the PR astral-sh/ruff#16611. But some parts I fixed are still usable.

The changes are as follows:

* fix `is_gradual_equivalent_to` for union/intersection types containing multiple dynamic types
* fix `union_elements_ordering` (implement lexicographic order comparison and use instead of automatic implementation)
* implement special handlings of `is_subtype_of`, `is_assignable_to` for simplified types such as `LiteralString & AlwaysFalsy -> LiteralString & Literal[“”]`
* fix a boolean simplification bug in `IntersectionBuilder::add_positive`
* improve handling of `is_disjoint_from` for negative intersection types

## Test Plan

With these changes, the property tests `all_fully_static_type_pairs_are_supertypes_of_their_intersection` and `all_type_pairs_can_be_assigned_from_their_intersection` now succeed in almost all cases (10M attempts were run for each and passed). If you agree that this problem is completely fixed, I will move these two tests from `flaky` to `stable`.

---

_Review requested from @carljm by @mtshiba on 2025-03-11 18:16_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-11 18:16_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-11 18:16_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-11 18:16_

---

_Comment by @github-actions[bot] on 2025-03-11 18:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-03-11 18:27_

Thank you! Haven't fully reviewed yet, just a quick note from running property tests on this PR. This also appears to fix a couple stable property tests I'd been looking at earlier this morning that were newly failing since astral-sh/ruff#16611 landed (`all_type_pairs_are_assignable_to_their_union` and `subtype_of_implies_assignable_to`).

I am seeing a new failure of the stable property test `disjoint_from_is_symmetric` with this PR, though. Example type pair: 

```
thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [KnownClassInstance(Tuple)] }, Intersection { pos: [], neg: [Tuple([BuiltinsFunction("chr"), BuiltinsFunction("ascii")])] })
```

---

_Comment by @carljm on 2025-03-11 18:32_

I am seeing that `all_fully_static_type_pairs_are_supertypes_of_their_intersection` and `all_type_pairs_can_be_assigned_from_their_intersection` are both consistently passing with this PR, so I think you are good to move those over to stable!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:700 on 2025-03-11 18:37_

Are we also able to detect disjointness of these type pairs? If so it might be useful to also add these cases to `type_properties/is_disjoint_from.md` in the form `static_assert(is_disjoint_from(..., ...))`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:312 on 2025-03-11 19:19_

These three tests all look like they are describing a type equivalence, not just a subtype relationship. It seems they all pass if modified to use `is_equivalent_to` instead. Maybe we should also add these tests to `type_properties/is_equivalent_to.md`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:319 on 2025-03-11 19:19_

These also look like type equivalences.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:329 on 2025-03-11 19:20_

These also look like equivalencies?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:277 on 2025-03-11 19:24_

Would it be more efficient to do this check first, to avoid lots of element comparisons if we compare two lengthy unions of different lengths?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:299 on 2025-03-11 19:24_

Same as above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3832 on 2025-03-11 19:36_

I think perhaps we should just always do this eagerly in the union and intersection builders? Though there may be cases, particularly with Todo types, where this reduces debuggability. We can look into this separately.

---

_@carljm reviewed on 2025-03-11 19:39_

This looks like a positive step to me! I think we should figure out and solve the disjointness asymmetry that it seems to introduce, but otherwise I'm happy to go with it for now.

I think this effectively solves https://github.com/astral-sh/ruff/issues/15513, in a more special-cased way than we were initially aiming for there. But I think that's fine; literal types are special cases.

---

_Label `red-knot` added by @carljm on 2025-03-11 20:43_

---

_Comment by @carljm on 2025-03-11 20:54_

This PR has some overlap with https://github.com/astral-sh/ruff/pull/16636. As I mentioned there, they both look generally good to me, I will merge whichever one first reaches merge-ready state, and the other can rebase and see what is left to add.

---

_Comment by @jgeralnik on 2025-03-11 22:57_

For what it's worth I made the same change to intersections in is_disjoint over in astral-sh/ruff#16636 and it has the same asymmetry problem as this commit. I haven't yet managed to make a version that passes all four of these tests:

```
class X: ...
class Y: ...

static_assert(not is_disjoint_from(Intersection[Any, X], Intersection[Any, Not[Y]]))
static_assert(not is_disjoint_from(Intersection[Any, Not[Y]], Intersection[Any, X]))

static_assert(is_disjoint_from(Intersection[int, Any], Not[int]))
static_assert(is_disjoint_from(Not[int], Intersection[int, Any]))
```

---

_Comment by @codspeed-hq[bot] on 2025-03-14 18:14_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A14899-gradual-type-assignability)

### Merging astral-sh/ruff#16641 will **not alter performance**

<sub>Comparing <code>mtshiba:14899-gradual-type-assignability</code> (9718536) with <code>main</code> (1fab292)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_@AlexWaygood reviewed on 2025-03-14 18:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:1 on 2025-03-14 18:31_

The title of this PR is "Stabilize `all_type_pairs_can_be_assigned_from_their_intersection`", but it seems like on this PR branch, it's still in the `flaky` submodule in this file. In order for it to be marked as stable, you need to move it to the `stable` submodule in this file. If you do so, it will be run once a day automatically as part of a cron job, and a bot will open an issue like [this one](https://github.com/astral-sh/ruff/issues/16670) when the tests fail. (These tests aren't run normally as part of our CI since they're quite slow!)

In actual fact, it looks like the changes made in this PR mean that _four_ property tests currently in the `flaky` submodule can be moved to the `stable` submodule:
- `all_type_pairs_can_be_assigned_from_their_intersection`
- `intersection_equivalence_not_order_dependent`
- `negation_is_disjoint`
- `all_fully_static_type_pairs_are_supertypes_of_their_intersection`


---

_Comment by @mtshiba on 2025-03-14 18:40_

Yes, it seems to pass several tests other than the mentioned one in the PR title.

However, many branches are added to handle `is_subtype_of/is_assignable_to` correctly. There seems to be some performance issues as well. I am looking for ideas for improvement, but I don't think this way is the final, best solution. Please let me know what you all think.

If this is not desirable, or if a good fix is in progress, I will revert to a commit with the test cases marked as TODO, leaving only the correct bug fix part of the PR.

---

_@AlexWaygood reviewed on 2025-03-14 18:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:1 on 2025-03-14 18:44_

Oops, I spoke too soon. `intersection_equivalence_not_order_dependent` is still flaky with this PR. I thought it was now stable as I ran it with `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::flaky::intersection_equivalence_not_order_dependent` and it didn't fail once... but then I ran it again and it immediately produced this failure 🙃

```
failures:

---- types::property_tests::stable::intersection_equivalence_not_order_dependent stdout ----

thread 'types::property_tests::stable::intersection_equivalence_not_order_dependent' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([KnownClassInstance(Bool), AlwaysTruthy]), Intersection { pos: [], neg: [] }, Union([AlwaysTruthy, AlwaysFalsy]))
```

---

_@AlexWaygood reviewed on 2025-03-14 19:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:284 on 2025-03-14 19:05_

these Salsa lookups (the methods that take `db`) can be surprisingly expensive. It would be better to do something like this (and similar for the `Type::Intersection` branch below this):

```suggestion
        (Type::Union(left), Type::Union(right)) => {
            let left = left.elements(db);
            let right = right.elements(db);
            left.len().cmp(&right.len()).then_with(|| {
                left.iter()
                    .zip(right)
                    .map(|(left, right)| union_elements_ordering(left, right, db))
                    .find(|ordering| *ordering != Ordering::Equal)
                    .unwrap_or(Ordering::Equal)
            })
        }
```

but I also don't really understand why this change is necessary. I see that one mdtest fails if I revert the changes to this file:

```
failures:

---- mdtest__type_properties_is_gradual_equivalent_to stdout ----

is_gradual_equivalent_to.md - Gradual equivalence relation - Unions and intersections

  crates/red_knot_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md:45 unexpected error: [static-assert-error] "Static assertion error: argument evaluates to `False`"
```

But I don't really understand why. Can you explain it to me?

---

_Comment by @AlexWaygood on 2025-03-14 19:14_

> Yes, it seems to pass several tests other than the mentioned one in the PR title.
> 
> However, many branches are added to handle `is_subtype_of/is_assignable_to` correctly. There seems to be some performance issues as well. I am looking for ideas for improvement, but I don't think this way is the final, best solution. Please let me know what you all think.
> 
> If this is not desirable, or if a good fix is in progress, I will revert to a commit with the test cases marked as TODO, leaving only the correct bug fix part of the PR.

https://github.com/astral-sh/ruff/pull/16641#discussion_r1996132460 might help with performance.

https://github.com/astral-sh/ruff/pull/15784, https://github.com/astral-sh/ruff/pull/15773 and https://github.com/astral-sh/ruff/pull/15738 are all attempts at fixing _some_ of the problems fixed in this PR in a slightly more general way. It's possible that merging one of those might make this PR's changes redundant. But I need to get back to those; I set them aside for a while as other things seemed more important.

I'll look at this PR more in-depth over the weekend or on Monday if nobody beats me to it; I need to stop working for the day now :-)

---

_@mtshiba reviewed on 2025-03-14 19:23_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:284 on 2025-03-14 19:23_

The following test did not pass because the union elements were not aligned correctly.

```python
static_assert(is_gradual_equivalent_to(
    Intersection[str | int, Not[type[Any]]],
    Intersection[int | str, Not[type[Unknown]]]
))
```
The arguments of `is_gradual_equivalent_to` were evaluated as follows:

```python
LHS: (str & ~type[Any]) | (int & ~type[Any])
RHS: (int & ~type[Unknown]) | (str & ~type[Unknown])
```

If the union elements were sorted correctly, `is_gradual_equivalent_to` would work. like this:

```python
LHS: (int & ~type[Any]) | (str & ~type[Any])
RHS: (int & ~type[Unknown]) | (str & ~type[Unknown])
```

But they weren't. This means that there was a problem with ordering for intersections.

---

_@AlexWaygood reviewed on 2025-03-14 19:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:284 on 2025-03-14 19:26_

Thank you, that's really helpful! Makes total sense :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:286 on 2025-03-14 19:37_

Our representation of types is always in disjunctive normal form (union of intersections) and does not permit nested unions. Since this method is only used to sort elements of unions and intersections, it is impossible for us to ever encounter a union here. I think we should use `unreachable!` to assert this invariant, rather than adding dead code here.

```suggestion
        (Type::Union(_), _) | (_, Type::Union(_)) => {
            unreachable!("our type representation does not permit nested unions")
        }
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:24 on 2025-03-14 19:39_

Orthogonal to this PR, but this method is used to sort elements of both unions and intersections. The doc comment above describes this correctly, but this name is misleading. I would suggest we rename to `union_or_intersection_elements_ordering`. (It only has two call sites, so verbosity is not really an issue.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:284 on 2025-03-14 19:41_

I don't think _this_ change is necessary (because unions can never actually appear here, see my other comment), but the below change for intersections is necessary. The point is that intersections are a type that can appear within a union, and using just `cmp` on the vector of elements in the intersection (as we did before this PR) means that two intersections that should be considered the same can be considered different, and thus mess up the comparison of the outer union. As in @mtshiba 's example.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:307 on 2025-03-14 19:42_

I'm not convinced this is the source of the incremental-check regression (since I don't remember seeing the regression on earlier versions of the diff that included this code), but I do think it's probably worth extracting `{left,right}.negative(db)` and `{left,right}.positive(db)` into local variables so we call those queries just once each.

---

_@carljm reviewed on 2025-03-14 19:43_

Still reviewing, but submitting a few comments related to an active discussion.

---

_@carljm reviewed on 2025-03-14 19:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:1 on 2025-03-14 19:53_

Hmm, interesting. Empty intersection types cannot be created by our type builders; it will simplify to just `object`. So we should probably prevent an empty intersection from being constructed in the property tests, too. Maybe that will be enough to fix this property test?

---

_@carljm reviewed on 2025-03-14 19:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:1 on 2025-03-14 19:55_

Oh, never mind - we already use `IntersectionBuilder` in the property tests, it's just the examples are shown in the original `Ty` representation. So this is a real bug.

I ran it again and got another counter-example for it:

```
[quickcheck] TEST FAILED. Arguments: (KnownClassInstance(Int), Union([KnownClassInstance(Bool), AlwaysFalsy]), Intersection { pos: [], neg: [Intersection { pos: [], neg: [AlwaysFalsy, BooleanLiteral(true)] }] })
```

---

_@carljm reviewed on 2025-03-14 20:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:1 on 2025-03-14 20:07_

The root cause of these failures is that we simplify `(bool | AlwaysTruthy) & (AlwaysTruthy | AlwaysFalsy)` to `AlwaysTruthy | Literal[False]`, but if we swap that intersection around to `(AlwaysTruthy | AlwaysFalsy) & (bool | AlwaysTruthy)`, we only reduce it to `bool | AlwaysTruthy`. So this I guess demonstrates that the approach in this PR is not (so far) adequate to really resolve #15513, because the simplification we do in union/intersection builder is still order sensitive.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:576 on 2025-03-14 20:12_

```suggestion
                // `bool` is a subtype of any type that `Literal[True]` and `Literal[False]` are both subtypes of.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:590 on 2025-03-14 20:13_

```suggestion
                // `LiteralString` is a subtype of any type that both `Literal[""]` and `AlwaysTruthy` are subtypes of
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:606 on 2025-03-14 20:18_

We understand that `is_subtype_of(AlwaysFalsy, Not[AlwaysTruthy])`, so this double condition is redundant, all tests pass if it is simplified to this:
```suggestion
                else if self.is_subtype_of(db, Type::AlwaysTruthy.negate(db))
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:604 on 2025-03-14 20:22_

```suggestion
                // `Not[AlwaysTruthy]` is a subtype of any type that both `Literal[""]` and `Not[LiteralString]` are subtypes of
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:619 on 2025-03-14 21:11_

I think this is the special case that is most concerning from a performance perspective, since many types will be subtypes of `Not[AlwaysTruthy]`, even if they are totally unrelated to literal strings, and we will have to do these extra checks on all of those types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:742 on 2025-03-14 21:23_

This seems to effectively just undo https://github.com/astral-sh/ruff/pull/16636, which is unfortunate as that was a nice simplification. Is this really necessary?

---

_@carljm reviewed on 2025-03-14 21:27_

Haven't finished reviewing all the special cases but need to move on to other things. I think it would be good to split this PR apart. For instance I think the fix to ordering of intersections is good; can we land that separately? I'm not convinced by all the new special cases; it feels like some of them really should be handled in a more general way (like by improving our eager simplifications of types in intersection and union builder.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:307 on 2025-03-15 10:23_

> I'm not convinced this is the source of the incremental-check regression

FWIW, while there's still a regression on codspeed, it does look like https://github.com/astral-sh/ruff/pull/16641/commits/9718536fb866598b95b868c69db4265149770888 _significantly_ improved performance: https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A14899-gradual-type-assignability

---

_@AlexWaygood reviewed on 2025-03-15 10:23_

---

_Comment by @mtshiba on 2025-03-15 15:08_

Now I am convinced that this problem should be solved using a more general method. But it may be a good fact that at least half of the flaky tests will pass if we can implement the special equivalence relation for `LiteralString` and `bool` types.

For now, I would like to create a new PR that cut out the correct bug fix (`union_or_intersection_elements_ordering`) in this PR. The new test cases presented in this PR would be an option to add as TODOs, or reference for future work. Which would you prefer?

---

_Comment by @carljm on 2025-03-17 20:56_

> The new test cases presented in this PR would be an option to add as TODOs, or reference for future work. Which would you prefer?

I think it is useful to add reviewed test cases that should pass but don't, with TODO comments.

---

_Comment by @mtshiba on 2025-03-19 18:05_

OK, so I'll create a new PR with the test cases marked as TODOs and add them, then close this PR.

---

_Closed by @mtshiba on 2025-03-19 18:20_

---

_Branch deleted on 2025-03-24 14:32_

---
