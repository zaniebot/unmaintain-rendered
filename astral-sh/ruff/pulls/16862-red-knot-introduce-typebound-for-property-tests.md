```yaml
number: 16862
title: "[red-knot] Introduce `TypeBound` for property tests"
type: pull_request
state: closed
author: cake-monotone
labels:
  - ty
assignees: []
base: main
head: cake/introduce-arbitrary-rule-for-property-tests
created_at: 2025-03-20T06:57:07Z
updated_at: 2025-03-29T22:19:20Z
url: https://github.com/astral-sh/ruff/pull/16862
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Introduce `TypeBound` for property tests

---

_@cake-monotone_

## Summary

This feature was suggested by @AlexWaygood.

### Problem with the Previous Approach

In property tests with a premise, the test could not perform valid checks until it randomly generated a type that satisfied the premise. This led to unnecessary computational waste.

This was particularly problematic in tests with three arguments, significantly reducing the efficiency of generating valid test cases.

For example:

```rust
type_property_test!(
    union_equivalence_not_order_dependent, db,
    forall types s, t, u.
        s.is_fully_static(db) && t.is_fully_static(db) && u.is_fully_static(db) => <property>
);
```

### Improvements

The `type_property_test` macro now explicitly requires both the identifiers and their corresponding `ArbitraryRule`.

```rust
type_property_test!(
    intersection_equivalence_not_order_dependent, db,
    forall (s: FullyStaticTy, t: FullyStaticTy, u: FullyStaticTy).
        <property>
);
```

Now, `s`, `t`, and `u`—which are declared as `FullyStaticTy`—will only be generated from types that satisfy `ty.is_fully_static()`.

The rules `AnyTy`, `FullyStaticTy`, and `SingletonTy` each maintain their own random pools for generating `Ty` instances.
For more details, please refer to the changes in `arbitrary.rs`.


## Test Plan

A new `ensure_rules` module has been added to verify that the types generated under each rule satisfy the expected conditions.

You can run the following command to validate the implementation:

```sh
cargo test -p red_knot_python_semantic -- --ignored ensure_rules
```





---

_Review requested from @carljm by @cake-monotone on 2025-03-20 06:57_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-03-20 06:57_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-20 06:57_

---

_Review requested from @dcreager by @cake-monotone on 2025-03-20 06:57_

---

_Renamed from "Introduce `ArbitraryRule` for property tests" to "[red-knot] Introduce `ArbitraryRule` for property tests" by @cake-monotone on 2025-03-20 06:57_

---

_Comment by @github-actions[bot] on 2025-03-20 07:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-20 07:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:266 on 2025-03-20 22:13_

It looks to me like there are some fully-static types in the above list that are not included here, silently reducing our type coverage for tests using `FullyStaticTy`. (To pick just one example, `BuiltinClassLiteral('str')` is both fully-static and a singleton, but is not included either here or below.)

I think this is a significant drawback of this PR. Rather than having a single pool of candidate types, with the actual implemention of `is_fully_static` and `is_singleton` deciding which types fall into which categories, we now have to manually categorize the candidate types. And while the new `ensure_rules` tests help validate that we don't include any type in a category to which it doesn't belong, there's nothing helping us validate that we aren't silently losing coverage by omitting types wrongly.

If we do decide to go ahead with this PR, I would at least prefer if we could not repeat ourselves. That is, have a single list of non-fully-static types, a single list of static types, and a single list of singleton types. `FullyStaticTy` would combine the latter two lists, `AnyTy` would combine all three, `SingletonTy` would use only the latter list.

---

_@carljm reviewed on 2025-03-20 22:15_

Thank you for the PR!

I see a significant impact on an individual test using `SingletonTy` (~75% improvement), a smaller impact on tests using `FullyStaticTy` (~15% improvement), and a fairly small impact on overall runtime of the stable tests (~4% improvement). 

This perf improvement is nice, but perhaps not as large overall as I might have hoped for from this change, I think mostly because most of the tests still use `AnyTy` anyway.

Personally I am not sure this perf improvement is worth the coverage drawbacks I discuss in my inline comment. Would like to hear other opinions.

---

_@cake-monotone reviewed on 2025-03-21 03:18_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:266 on 2025-03-21 03:18_

First of all, I think I should explain that my initial intuition was off. I had assumed that using `is_fully_static` would make it very unlikely for valid tests—but it turns out that’s not really the case.

From my simulation, the probability that all three variables of `AnyTy` satisfy the `is_fully_static` premise is around 65%. That means, out of 100 test runs, about 65 actually test the property, which is higher than I expected.

The key point I was trying to focus on wasn’t performance—it was to remove meaningless tests. If the probability of satisfying a property’s premise approaches zero, the test becomes meaningless. This PR was based on that assumption, so the exact ratio of `AnyTy` to `FullyStaticTy` wasn’t my main concern.\
But as you've pointed out, `FullyStaticTy` doesn’t seem to be as effective as I initially thought.

In conclusion, I think it's totally fine to close this PR.

---

Here are a few more stats from the simulation, which might be helpful for future property test reviews:

- Probability that `t.is_singleton()` holds: \~9%
- Probability that `!t.is_fully_static()` holds: \~10%
- Probability that `s.is_equivalent_to(db, t) && t.is_equivalent_to(db, u)` holds: \~0.1%
- Probability that `s.is_gradual_equivalent_to(db, t)` holds: \~1%

These probabilities will likely decrease as we add more types to the random pool. But for now, it seems we can compensate by increasing the number of tests. If we move forward with similar PRs in the future, I think we should aim to eliminate premises with very low probability, not fully static.


---

_@carljm reviewed on 2025-03-21 18:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:266 on 2025-03-21 18:57_

Aha, I hadn't fully processed the fact that if we have a premise of low probability, that doesn't just impact performance of the test, it also impacts usefulness of the test; a premise of 10% probability means we need 10x the `QUICKCHECK_TESTS` value to get the same number of actual tests of the implication. I agree this is a stronger motivation for the PR, and could be worth the risk of missing coverage of some type that we fail to include in the right list.

I would consider going ahead with this PR if we implement the above suggestion to avoid repetition in the type lists (so each type is listed exactly once, making it easier to evaluate whether each type is in the right place). We could also consider eliminating `FullyStaticTy` and just separating `SingletonTy`, but I kind of think if we go down this route we may as well do it fully.

But I would also like to hear @sharkdp 's opinion, so maybe no need to do any updates here until he is back.

---

_@sharkdp reviewed on 2025-03-24 15:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:266 on 2025-03-24 15:10_

This PR looks great! It's something I've been meaning to implement as well. I kind of liked the generic and simple `premise => property` form, but with the syntactic change to the macro, this reads even better, I think.

> with the actual implemention of `is_fully_static` and `is_singleton` deciding which types fall into which categories, we now have to manually categorize the candidate types.

Maybe we can maintain that property? We could have a single pool of types and then categorize them into (static, `LazyLock`) specialized lists for usage in those dedicated `Arbitrary` implementations?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:47 on 2025-03-24 15:13_

I liked the mathematical `.` here in the syntax, but it's not like it's particularly important, if it's difficult to keep.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:44 on 2025-03-24 15:14_

```suggestion
/// - `<property>` is an expression using these identifiers
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:60 on 2025-03-24 15:15_

Why did this become a problem just now?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:9 on 2025-03-24 15:18_

Maybe call this `BoundedType` or similar? It represents an arbitrary type that is bound by a constraint. And `ArbitraryRule` could be a `TypeBound`. There might be a small risk for confusion with type variable bounds, but they're quite similar concepts, so it might be fine?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:19 on 2025-03-24 15:20_

Maybe add a constructor so we can avoid these `_marker: …` lines?

---

_@sharkdp reviewed on 2025-03-24 15:25_

Thank you for working on this!

---

_@cake-monotone reviewed on 2025-03-25 04:18_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests.rs`:60 on 2025-03-25 04:18_

It’s because of the macro for `$premise`.
Since it has been generating `!($premise) || ($conclusion)`, I think the `||` operator affects the evaluation.

---

_@cake-monotone reviewed on 2025-03-25 04:31_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/arbitrary.rs`:266 on 2025-03-25 04:31_

Sounds good! I’ll go ahead with the PR.

As sharkdp suggested, I think keeping a single pool and letting the `Arbitrary` implementations handle the categorization makes sense — it should make it easier to support more premises in the future.

---

_@cake-monotone reviewed on 2025-03-25 04:32_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests.rs`:47 on 2025-03-25 04:32_

Sure, no problem! I’ll go ahead and change it.

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-26 05:25_

The types returned by `generate_singular_type `are guaranteed to satisfy a predicate (e.g., `is_fully_static`).
However, the result of `generate_type_recursively` may not necessarily satisfy it.
One option is to keep generating random types until the condition is met, but I'm not sure if that's a good approach.

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:122 on 2025-03-26 05:27_

Same issue with `generate_type_recursively`; the reulst may not satisfy a predicate.

---

_@cake-monotone reviewed on 2025-03-26 05:28_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-26 05:29_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-26 13:03_

For `is_fully_static` specifically, that should not be a problem, right? Unions, intersections, and tuples of fully static types are still fully static. And for singleton types, you special-cased `generate_type_recursively`. So I'm assuming you are worried about future extensions?

Maybe it would make sense to add something like a `check_property :: Ty -> bool` function to this trait, which we could then use as a post-condition assertion for the return type of `generate_type_recursively` in `BoundedType<Bound>::arbitrary` before feeding a type to the actual property test?


---

_@sharkdp reviewed on 2025-03-26 13:03_

---

_@sharkdp reviewed on 2025-03-26 13:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:244 on 2025-03-26 13:06_

This represents an arbitrary gradual type. Should we maybe rename it to `GradualType`/`GradualTy` to avoid any confusion with `typing.Any`? 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-26 13:27_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-27 07:23_

You're right — it's not a problem for FullyStaticTy and SingletonTy. The issue came up while I was working on NonFullyStaticTy, where an empty tuple was being generated unexpectedly. I'll apply your suggestion and commit it for now. Thanks!

---

_@cake-monotone reviewed on 2025-03-27 07:23_

---

_Renamed from "[red-knot] Introduce `ArbitraryRule` for property tests" to "[red-knot] Introduce `TypeBound` for property tests" by @cake-monotone on 2025-03-27 07:30_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-27 07:31_

---

_@sharkdp reviewed on 2025-03-27 21:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-27 21:54_

> The issue came up while I was working on NonFullyStaticTy, where an empty tuple was being generated unexpectedly.

I see. Is this why you implemented it as a re-try mechanism now instead of a hard assertion? I wonder if we could turn this into a hard assertion if we would specialize the tuple generation for this specific type? To avoid having to overload `generate_type_recursively` completely, we could potentially split it up into four functions (five soon, when we [add `Callable`s](https://github.com/astral-sh/ruff/pull/17006)), and then only specialize the tuple-generation part?

I wonder if we also need to take care of this for shrinking? Could we currently accidentally shrink a non-fully static counterexample type to an empty tuple (which would not fulfill the precondition of the test anymore)? And then erroneously report the empty tuple as a counterexample to a test that requires non-fully static types as inputs?

---

_@cake-monotone reviewed on 2025-03-28 02:32_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-28 02:32_

> I wonder if we could turn this into a hard assertion if we would specialize the tuple generation for this specific type?

Ah, I see — that does sound like a better approach. I’ll give it a try!

> Could we currently accidentally shrink a non-fully static counterexample type to an empty tuple (which would not fulfill the precondition of the test anymore)?

While it's pretty clear that this edge case won’t happen at the moment, I think it’s still something that could totally happen down the line, depending on how the type system evolves. So I think it’s worth ensuring we guard against it now. 
 In conclusion I think just filtering in the Arbitrary shrink, like in this PR, should be sufficient

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-28 03:42_

Hmm... it's not quite as simple as I thought.

For `NonFullyStaticTy`, there's another counterexample: `Intersection`.

`Ty::Intersection` uses `IntersectionBuilder` to generate a `Type`, so the result isn't always an actual intersection type. For example:

- object instance from `Ty::Intersection(pos: [], neg: [])`
- `Type::Never` from cases like:

```rust
Ty::Intersection {
    pos: vec![
        Ty::Tuple(vec![Ty::SubclassOfAny]),
        Ty::Tuple(vec![Ty::Unknown, Ty::Any]),
    ],
    neg: vec![],
}
```

...and many more combinations.

So it’s hard to determine whether the result of `generate_type_recursively` satisfies the precondition, and overloading it probably won’t help here.


---

_@cake-monotone reviewed on 2025-03-28 03:42_

---

_@sharkdp reviewed on 2025-03-28 15:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-28 15:15_

To be really honest, I'm starting to think if we should just leave everything as is? This is a significant increase in code complexity and we have identified a few pitfalls that might turn out to cause problems in the future. What is your impression, @cake-monotone?

---

_@cake-monotone reviewed on 2025-03-29 13:27_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-29 13:27_

I totally agree — I had the same thought when I realized this structure keeps introducing more complexity.
Maybe the main takeaway from this PR is that we need to be more mindful of the premises when creating or reviewing property tests.

Going back to the original goal; the main purpose of this PR was to improve the valid-testing rate of property test, right? So let's simply double the `QUICKCHECK_TEST`, since we already got a 75% performance boost from the earlier PR.

Let's close this PR for now!


---

_@sharkdp reviewed on 2025-03-29 22:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests/type_bound.rs`:117 on 2025-03-29 22:19_

Ok — Thank you very much for performing this experiment!

---

_Closed by @sharkdp on 2025-03-29 22:19_

---
