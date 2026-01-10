```yaml
number: 13962
title: "[red-knot] Distribute intersections on negation"
type: pull_request
state: merged
author: rtpg
labels:
  - ty
assignees: []
merged: true
base: main
head: negation-logic
created_at: 2024-10-28T10:59:06Z
updated_at: 2024-11-01T13:32:07Z
url: https://github.com/astral-sh/ruff/pull/13962
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Distribute intersections on negation

---

_Pull request opened by @rtpg on 2024-10-28 10:59_

## Summary

This does two things:
- distribute negated intersections when building up intersections (i.e. going from `A & ~(B & C)` to `(A & ~B) | (A & ~C)`) (fixing #13931) 
- add some classes from `typing` to `KnownClass`

The second point was to enable writing a test for the first case using protocols defined in `typing` (here, `Sized` and `Hashable`). Was a bit worried about another test that could be susceptible to simplifications, and it was straightforward to pull in

## Test Plan

`cargo test`

---

_Review requested from @carljm by @rtpg on 2024-10-28 10:59_

---

_Review requested from @MichaReiser by @rtpg on 2024-10-28 10:59_

---

_Review requested from @AlexWaygood by @rtpg on 2024-10-28 10:59_

---

_Comment by @rtpg on 2024-10-28 11:54_

Sorry, in my rush to send this in with the test I added passing I had intro'd a regression on another test. Please ignore this PR until I get this green (unless you know what's actually wrong here of course)

---

_Converted to draft by @MichaReiser on 2024-10-28 11:56_

---

_Comment by @MichaReiser on 2024-10-28 11:57_

@rtpg no worries. I'll convert the PR to a draft and you can mark it as ready when it's ready :)

---

_@TomerBin reviewed on 2024-10-28 19:11_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types/builder.rs`:404 on 2024-10-28 19:11_

I think that you can use `Type::negate` 

---

_Comment by @github-actions[bot] on 2024-10-28 23:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @rtpg on 2024-10-29 00:12_

---

_Comment by @rtpg on 2024-10-29 00:12_

Alright this one is ready for review, all green etc.

---

_@rtpg reviewed on 2024-10-29 00:25_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/builder.rs`:691 on 2024-10-29 00:25_

a part of me wants something like `Type::intersection(elts: Vec<Type<'db>>)` for testing but I got pretty used to writing out the intersection builder at this point 😆 

---

_@carljm reviewed on 2024-10-29 00:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:691 on 2024-10-29 00:28_

Ha, you aren't the first to say this after working on IntersectionBuilder, cc @sharkdp 😆 It's not a problem at all to add test helpers to make tests less verbose, someone just needs to feel the pain sufficiently to actually do it!

---

_@rtpg reviewed on 2024-10-29 00:45_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1247 on 2024-10-29 00:45_

basically all of the "interesting" protocols in `typing` are deprecated aliases at this point to members of `collections.abc`. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/stdlib.rs`:70 on 2024-10-29 02:05_

```suggestion
/// Returns `Unbound` if the `typing` module isn't available for some reason.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1140 on 2024-10-29 02:10_

Sorry to catch this only after you'd done the work, but I don't see any reason for these to be known classes? We should only use `KnownClass` when either a) the type has special-cased behavior that is not reflected in its typeshed definition, or b) we will commonly need to conjure this type "from thin air" within the type inference code itself, usually because the type is builtin and sometimes created "from thin air" by the runtime (e.g. `bool`) or because it is closely related to other special-cased types (e.g. `KnownClass::Int` and `Type::IntLiteral`, etc.)

I don't think `Sized` or `Hashable` meet either of those criteria.

If you want to use these types (or any other typeshed type) in tests, you don't need it to be a `KnownClass`; that's just a minor convenience. You can just use `typing_symbol_ty` (added in this PR, and something we'll definitely want) directly in the test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:594 on 2024-10-29 02:14_

```suggestion
        // (~Any is equivalent to  Any)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:597 on 2024-10-29 02:15_

Confusing to name this intersection when it is actually a union :)

```suggestion
        let union = IntersectionBuilder::new(&db)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:609 on 2024-10-29 02:16_

```suggestion
        assert_eq!(union.elements(&db), &[i_and_a, t1,]);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:597 on 2024-10-29 02:16_

FWIW another option here is to just assert on the displayed form of the type, `build_negative_union_de_morgan` does that

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:664 on 2024-10-29 02:18_

```suggestion
        // s_not_h_t: Sized & ~Hashable
```

---

_@carljm requested changes on 2024-10-29 02:18_

Looks great, thank you! Just a few suggested changes, main one being to remove `Sized` and `Hashable` from `KnownClass`

---

_@rtpg reviewed on 2024-10-29 02:33_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1140 on 2024-10-29 02:33_

I hadn't quite internalized that I only actually really need `typing_symbol_ty`, thanks for pointing that out. Will remove  the`KnownClass` entries 

---

_@rtpg reviewed on 2024-10-29 02:34_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/builder.rs`:597 on 2024-10-29 02:34_

At first wasn't a fan because of the display issues, but given it's DNF (I belive) it's unambiguous... will do that

---

_@carljm approved on 2024-10-29 02:54_

Looks good, thank you!!

---

_Merged by @carljm on 2024-10-29 02:56_

---

_Closed by @carljm on 2024-10-29 02:56_

---

_Label `red-knot` added by @dhruvmanila on 2024-11-01 13:32_

---
