```yaml
number: 15332
title: "[red-knot] all types are assignable to object"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/assign-to-object
created_at: 2025-01-07T21:21:34Z
updated_at: 2025-01-07T23:19:09Z
url: https://github.com/astral-sh/ruff/pull/15332
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] all types are assignable to object

---

_Pull request opened by @carljm on 2025-01-07 21:21_

## Summary

`Type[Any]` should be assignable to `object`. All types should be assignable to `object`.

We specifically didn't understand the former; this PR adds a test for it, and a case to ensure that `Type[Any]` is assignable to anything that `type` is assignable to (which includes `object`).

This PR also adds a property test that all types are assignable to object. In order to make it pass, I added a special case to check early if we are assigning to `object` and just return `true`. In principle, once we get all the more general cases correct, this special case might be removable. But having the special case for now allows the property test to pass.

And we add a property test that all types are subtypes of object. This failed for the case of an intersection with no positive elements (that is, a negation type). This really does need to be a special case for `object`, because there is no other type we can know that a negation type is a subtype of.

## Test Plan

Added unit test and property test.


---

_Label `red-knot` added by @carljm on 2025-01-07 21:21_

---

_Review requested from @MichaReiser by @carljm on 2025-01-07 21:21_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-07 21:21_

---

_Review requested from @sharkdp by @carljm on 2025-01-07 21:21_

---

_Comment by @github-actions[bot] on 2025-01-07 21:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:301 on 2025-01-07 21:47_

Is it worth also adding a subtyping version of this for fully static types?

```suggestion
    );

    // And for fully static types, they should also be subtypes of `object`
    type_property_test!(
        all_fully_static_types_subtype_of_object, db,
        forall types t. t.is_fully_static() => t.is_subtype_of(db, KnownClass::Object.to_instance(db))
    );
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:996 on 2025-01-07 21:58_

I've spent a while staring at this branch now 😆 I _think_ it's correct, it just took me a little bit of time to wrap my head around it -- adding some comments might be good. It's saying that `type[Any]` is a consistent subtype of a type `T` if `T` is either a consistent subtype or a consistent supertype of `type`. Right?

---

_@AlexWaygood approved on 2025-01-07 21:58_

---

_@AlexWaygood reviewed on 2025-01-07 22:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:300 on 2025-01-07 22:16_

oops, you need a db here

```suggestion
        forall types t. t.is_fully_static(db) => t.is_subtype_of(db, KnownClass::Object.to_instance(db))
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:300 on 2025-01-07 22:17_

Yes, I have it fixed locally, just working on another bug revealed by this property test :)

---

_@carljm reviewed on 2025-01-07 22:17_

---

_@sharkdp reviewed on 2025-01-07 22:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:996 on 2025-01-07 22:22_

> I've spent a while staring at this branch now

So glad I'm not the only one. I attributed it to tiredness :smile: 

---

_@carljm reviewed on 2025-01-07 22:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:996 on 2025-01-07 22:30_

I also found it confusing, both in its form prior to this change, and still after this change :) I think it's correct, though. I'll try to formulate a clear argument for why it is correct, and put that in a comment.

---

_@AlexWaygood reviewed on 2025-01-07 22:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:778 on 2025-01-07 22:40_

Oh, nice!! Yet another win for the property tests 😃

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:779 on 2025-01-07 22:42_

Subtype-of-object really does have to be a special case for negation types (that is, intersections with no positive elements). They are subtypes of `object` simply because `object` is the top type, so they must be, but there is no other type we can establish them to be a subtype of (other than another negation type, which is handled in the next case below.)

This case could be generalized to apply to any `self` type, not just intersections, and maybe it should be. I kind of like the fact that in most cases we don't need to special case `object` to get the right behavior (and the property test suggests we don't need to special case it for anything else). But practically speaking it might be faster to always shortcut for it.

---

_@carljm reviewed on 2025-01-07 22:42_

---

_@AlexWaygood reviewed on 2025-01-07 22:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:779 on 2025-01-07 22:44_

Conceptually it feels more elegant to me to fallback to the supertype where possible and let the handling for `object` fall out naturally for everything except this branch. But obviously if it is more performant, we should do the more performant thing.

---

_@AlexWaygood reviewed on 2025-01-07 22:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:779 on 2025-01-07 22:46_

> This case could be generalized to apply to any `self` type, not just intersections, and maybe it should be.

We did actually do this before https://github.com/astral-sh/ruff/pull/14924, but I took it out as part of that PR because (so I thought) we didn't need it anymore

---

_@carljm reviewed on 2025-01-07 23:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:996 on 2025-01-07 23:06_

I pushed comments for every case in `is_assignable_to`, including this one.

I found it pretty easy to justify the new clause I added here in this PR; `type[Any]` is assignable to anything `type[object]` is assignable to, because `type[Any]` can materialize to `type[object]`.

What I found harder to formulate a justification for was the previous clause that was already here, which said that `type[Any]` is assignable to anything which is assignable to `type[object]`. I found it clearer if I tightened that to "`type[Any]` is assignable to anything which is a subtype of `type[object]`", and that form still passes all tests, including property tests.

---

_@carljm reviewed on 2025-01-07 23:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:779 on 2025-01-07 23:07_

I'm not going to generalize it in this PR; we can explore it as a possible perf optimization later.

---

_@AlexWaygood approved on 2025-01-07 23:15_

---

_Merged by @carljm on 2025-01-07 23:19_

---

_Closed by @carljm on 2025-01-07 23:19_

---

_Branch deleted on 2025-01-07 23:19_

---
