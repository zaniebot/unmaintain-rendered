```yaml
number: 17451
title: "[red-knot] fix building unions with literals and AlwaysTruthy/AlwaysFalsy"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unionfix
created_at: 2025-04-17T17:32:21Z
updated_at: 2025-04-19T17:06:23Z
url: https://github.com/astral-sh/ruff/pull/17451
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] fix building unions with literals and AlwaysTruthy/AlwaysFalsy

---

_@carljm_

In #17403 I added a comment asserting that all same-kind literal types share all the same super-types. This is true, with two notable exceptions: the types `AlwaysTruthy` and `AlwaysFalsy`. These two types are super-types of some literal types within a given kind and not others: `Literal[0]`, `Literal[""]`, and `Literal[b""]` inhabit `AlwaysFalsy`, while other literals inhabit `AlwaysTruthy`.

This PR updates the literal-unions optimization to handle these types correctly.

Fixes https://github.com/astral-sh/ruff/issues/17447

Verified locally that `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` now passes again.

---

_Label `red-knot` added by @carljm on 2025-04-17 17:32_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-17 17:32_

---

_Review requested from @sharkdp by @carljm on 2025-04-17 17:32_

---

_Review requested from @dcreager by @carljm on 2025-04-17 17:32_

---

_Comment by @github-actions[bot] on 2025-04-17 17:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-04-18 15:19_

I'm going to go ahead and land this sans review, to get the property tests green again. Post-land review welcome!

---

_Merged by @carljm on 2025-04-18 15:20_

---

_Closed by @carljm on 2025-04-18 15:20_

---

_Branch deleted on 2025-04-18 15:20_

---

_@dcreager reviewed on 2025-04-18 15:40_

Looks good!

---

_Comment by @dcreager on 2025-04-18 15:45_

This added a test regression on `main`, since the new mdtests use a `type` statement, which isn't available until 3.12.  I've got a fix (https://github.com/astral-sh/ruff/pull/17434/commits/0b116e5daa81131e194b35b9b0177bd94a7ca575) ready to go on https://github.com/astral-sh/ruff/pull/17434, which I am about to merge once CI (hopefully) goes green.

---

_Comment by @carljm on 2025-04-18 15:47_

Huh, why didn't that show up on the PR CI? Did we recently change our default Python version on main? Thanks for fixing!

---

_Comment by @AlexWaygood on 2025-04-18 15:48_

Just a merge race with https://github.com/astral-sh/ruff/commit/9c47b6dbb0f29369fdf38d5048276994b2e09e49, I think!

---

_Comment by @carljm on 2025-04-18 15:48_

Oh right, of course!

---

_Comment by @dcreager on 2025-04-18 15:50_

> Huh, why didn't that show up on the PR CI? Did we recently change our default Python version on main? Thanks for fixing!

@ntBre added version-related syntax errors after the point on `main` where you had branched from: https://github.com/astral-sh/ruff/pull/16379

---

_Comment by @AlexWaygood on 2025-04-19 16:56_

@carljm I'm not sure this PR fixed things. https://github.com/astral-sh/ruff/issues/17478 seems to be this same issue repeating itself :(

> Verified locally that `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` now passes again.

Huh -- if I checkout this commit, the property tests still fail for me locally. 100,000 might not be enough iterations to reliably check whether the tests are likely to be flaky -- I usually run them with `QUICKCHECK_TESTS=1000000` these days:

```
HEAD is now at 84d064a14 [red-knot] fix building unions with literals and AlwaysTruthy/AlwaysFalsy (#17451)
~/dev/ruff (84d064a14)⚡ % QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable
   Compiling ruff_python_ast v0.0.0 (/Users/alexw/dev/ruff/crates/ruff_python_ast)
   Compiling red_knot_python_semantic v0.0.0 (/Users/alexw/dev/ruff/crates/red_knot_python_semantic)
   Compiling ruff_python_parser v0.0.0 (/Users/alexw/dev/ruff/crates/ruff_python_parser)
   Compiling ruff_python_literal v0.0.0 (/Users/alexw/dev/ruff/crates/ruff_python_literal)
   Compiling ruff_db v0.0.0 (/Users/alexw/dev/ruff/crates/ruff_db)
   Compiling red_knot_vendored v0.0.0 (/Users/alexw/dev/ruff/crates/red_knot_vendored)
   Compiling red_knot_test v0.0.0 (/Users/alexw/dev/ruff/crates/red_knot_test)
    Finished `release` profile [optimized] target(s) in 24.64s
     Running unittests src/lib.rs (target/release/deps/red_knot_python_semantic-bb028dec96527f17)

running 23 tests
test types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union ... FAILED
test types::property_tests::stable::all_type_pairs_are_assignable_to_their_union ... FAILED
test types::property_tests::stable::all_fully_static_types_subtype_of_object has been running for over 60 seconds
test types::property_tests::stable::all_types_assignable_to_object has been running for over 60 seconds
test types::property_tests::stable::assignable_to_is_reflexive has been running for over 60 seconds
test types::property_tests::stable::disjoint_from_is_irreflexive has been running for over 60 seconds
test types::property_tests::stable::disjoint_from_is_symmetric has been running for over 60 seconds
test types::property_tests::stable::equivalent_to_is_reflexive has been running for over 60 seconds
test types::property_tests::stable::equivalent_to_is_symmetric has been running for over 60 seconds
test types::property_tests::stable::equivalent_to_is_transitive has been running for over 60 seconds
test types::property_tests::stable::gradual_equivalent_to_is_symmetric has been running for over 60 seconds
test types::property_tests::stable::never_assignable_to_every_type has been running for over 60 seconds
test types::property_tests::stable::never_subtype_of_every_fully_static_type has been running for over 60 seconds
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence has been running for over 60 seconds
test types::property_tests::stable::never_assignable_to_every_type ... ok
test types::property_tests::stable::equivalent_to_is_reflexive ... ok
test types::property_tests::stable::never_subtype_of_every_fully_static_type ... ok
test types::property_tests::stable::assignable_to_is_reflexive ... ok
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping has been running for over 60 seconds
test types::property_tests::stable::disjoint_from_is_irreflexive ... ok
test types::property_tests::stable::all_fully_static_types_subtype_of_object ... ok
test types::property_tests::stable::all_types_assignable_to_object ... ok
test types::property_tests::stable::singleton_implies_single_valued has been running for over 60 seconds
test types::property_tests::stable::singleton_implies_single_valued ... ok
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_equivalence ... ok
test types::property_tests::stable::gradual_equivalent_to_is_symmetric ... ok
test types::property_tests::stable::equivalent_to_is_symmetric ... ok
test types::property_tests::stable::subtype_of_is_reflexive ... ok
test types::property_tests::stable::subtype_of_implies_assignable_to has been running for over 60 seconds
test types::property_tests::stable::subtype_of_implies_not_disjoint_from has been running for over 60 seconds
test types::property_tests::stable::subtype_of_is_antisymmetric has been running for over 60 seconds
test types::property_tests::stable::non_fully_static_types_do_not_participate_in_subtyping ... ok
test types::property_tests::stable::subtype_of_is_transitive has been running for over 60 seconds
test types::property_tests::stable::disjoint_from_is_symmetric ... ok
test types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent has been running for over 60 seconds
test types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent has been running for over 60 seconds
test types::property_tests::stable::equivalent_to_is_transitive ... ok
test types::property_tests::stable::subtype_of_implies_assignable_to ... ok
test types::property_tests::stable::subtype_of_is_antisymmetric ... ok
test types::property_tests::stable::subtype_of_implies_not_disjoint_from ... ok
test types::property_tests::stable::two_equivalent_types_are_also_gradual_equivalent ... ok
test types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent ... ok
test types::property_tests::stable::subtype_of_is_transitive ... ok

failures:

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([BytesLiteral("\0"), BytesLiteral("")]), Intersection { pos: [], neg: [BytesLiteral("")] })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([BytesLiteral("\0"), BytesLiteral("")]), Intersection { pos: [], neg: [AlwaysFalsy] })


failures:
    types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union
    types::property_tests::stable::all_type_pairs_are_assignable_to_their_union

test result: FAILED. 21 passed; 2 failed; 0 ignored; 0 measured; 210 filtered out; finished in 162.30s

error: test failed, to rerun pass `-p red_knot_python_semantic --lib`
```

---
