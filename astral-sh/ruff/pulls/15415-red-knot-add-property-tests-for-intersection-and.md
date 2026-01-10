```yaml
number: 15415
title: "[red-knot] Add Property Tests for Intersection and Union"
type: pull_request
state: merged
author: cake-monotone
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: red-knot-add-more-tests-to-property-tests
created_at: 2025-01-11T07:48:34Z
updated_at: 2025-01-12T13:22:44Z
url: https://github.com/astral-sh/ruff/pull/15415
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Add Property Tests for Intersection and Union

---

_Pull request opened by @cake-monotone on 2025-01-11 07:48_

## Summary

Closes #14986

### stable tests
- `all_fully_static_type_pairs_are_subtype_of_their_union`

### flaky tests
- `negation_is_disjoint`
- `all_fully_static_type_pairs_are_supertypes_of_their_union`
- `all_type_pairs_can_be_assigned_from_their_intersection`
- `all_type_pairs_are_assignable_to_their_union`

The tests that are currently failing have been categorized as flaky. The failure of `fully_static_types_are_supertypes_of_their_intersection` seems a bit unusual, so I’ll take look into it and create a separate issue if needed. :)

## Test Plan

stable: `QUICKCHECK_TESTS=10000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`
flaky: `QUICKCHECK_TESTS=10000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::flaky`

---

_Review requested from @carljm by @cake-monotone on 2025-01-11 07:48_

---

_Review requested from @MichaReiser by @cake-monotone on 2025-01-11 07:48_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-01-11 07:48_

---

_Review requested from @sharkdp by @cake-monotone on 2025-01-11 07:48_

---

_Comment by @github-actions[bot] on 2025-01-11 07:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:319 on 2025-01-11 12:44_

Let's use boolean and (`&&`) rather than bitwise and `&` :-)

```suggestion
            s.is_fully_static(db) && t.is_fully_static(db)
            => s.is_subtype_of(db, union(db, s, t)) & t.is_subtype_of(db, union(db, s, t))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:380 on 2025-01-11 12:45_

```suggestion
            s.is_fully_static(db) && t.is_fully_static(db)
            => intersection(db, s, t).is_subtype_of(db, s) && intersection(db, s, t).is_subtype_of(db, t)
```

---

_@AlexWaygood reviewed on 2025-01-11 12:46_

Brilliant, thank you!

---

_Label `testing` added by @AlexWaygood on 2025-01-11 13:15_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-11 13:15_

---

_@cake-monotone reviewed on 2025-01-11 13:56_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests.rs`:380 on 2025-01-11 13:56_

Thanks for catching it! 👍 !!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:320 on 2025-01-11 23:44_

Is it worth adding another test as well for assignability with non-fully-static types?

```suggestion
    // For fully static types, they should be subtype of their union.
    type_property_test!(
        fully_static_types_are_subtype_of_their_union, db,
        forall types s, t.
            s.is_fully_static(db) && t.is_fully_static(db)
            => s.is_subtype_of(db, union(db, s, t)) && t.is_subtype_of(db, union(db, s, t))
    );
    
    // And for *any* two types, even non-fully-static types,
    // each of the pair they should be assignable to the union of the two.
    type_property_test!(
        all_type_pairs_are_assignable_to_their_union, db,
        forall types s, t. s.is_assignable_to(db, union(db, s, t)) && t.is_assignable_to(db, union(db, s, t))
    );
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:381 on 2025-01-11 23:47_

And for non-fully-static types, the intersection should be assignable, I think:

```suggestion
    // For fully static types, their intersection should be subtype of them.
    type_property_test!(
        fully_static_types_are_supertypes_of_their_intersection, db,
        forall types s, t.
            s.is_fully_static(db) && t.is_fully_static(db)
            => intersection(db, s, t).is_subtype_of(db, s) && intersection(db, s, t).is_subtype_of(db, t)
    );

    // And for non-fully-static types, the intersection of a pair of types
    // should be assignable to both types of the pair.
    type_property_test!(
        fully_static_types_are_supertypes_of_their_intersection, db,
        forall types s, t. intersection(db, s, t).is_assignable_to(db, s) && intersection(db, s, t).is_assignable_to(db, t)
    );
```

---

_@AlexWaygood reviewed on 2025-01-11 23:49_

A couple of suggestions to also add some non-fully-static variations of these tests

---

_@cake-monotone reviewed on 2025-01-12 07:42_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests.rs`:320 on 2025-01-12 07:42_

That makes sense! I’ve added it. However, `all_type_pairs_are_assignable_to_their_union` was failing with high probability, so I moved it to the flaky module. It seems that `is_assignable_to` is not fully implemented for non-fully-static types.

---

_@AlexWaygood approved on 2025-01-12 13:17_

Thank you!

---

_Merged by @AlexWaygood on 2025-01-12 13:21_

---

_Closed by @AlexWaygood on 2025-01-12 13:21_

---
