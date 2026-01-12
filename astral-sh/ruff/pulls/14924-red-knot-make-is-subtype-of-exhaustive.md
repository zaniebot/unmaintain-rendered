```yaml
number: 14924
title: "[red-knot] Make `is_subtype_of` exhaustive"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/subtype-of-exhaustive
created_at: 2024-12-11T20:25:28Z
updated_at: 2024-12-13T19:34:49Z
url: https://github.com/astral-sh/ruff/pull/14924
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Make `is_subtype_of` exhaustive

---

_@AlexWaygood_

## Summary

This PR removes the fallback `_` branch at the end of `Type::is_subtype_of()`: the function now uses an exhaustive match over the first element of the `(self, target)` pair. This means that we can have much more confidence that we've covered all cases, and it means that it's impossible to forget to update this function if we add more variants to `Type` in the future (which we will).

The PR reworks our handling of most literal types in the `match` statement so that they explicitly fallback to builtin types. This has several advantages:
- We now understand that `Type::ModuleLiteral("typing")` is a subtype of `types.ModuleType`
- We now understand that `Type::SliceLiteral([:2])` is a subtype of `builtins.slice`
- We now understand that all heterogeneous tuple types are subtypes of `Type::Instance(Tuple)` (which represents the static set of all possible tuples, so in Python type annotations it would be written as `tuple[object, ...]`)
- We will naturally understand that all string literal types are subtypes of `typing.Sequence` as soon as we understand generics -- we won't have to make any manual updates to this function
- We can get rid of the explicit handling of `builtins.object` being the top type from the function; we now naturally understand this without any special-casing due to the fact that we understand that all classes have `object` in their MROs.

## Test Plan

Several new subtyping tests have been added, and all existing tests pass.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 20:26_

---

_Comment by @github-actions[bot] on 2024-12-11 20:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@InSyncWithFoo reviewed on 2024-12-11 20:34_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:639 on 2024-12-11 20:34_

Perhaps you mean "non-fully-static types"?

---

_@AlexWaygood reviewed on 2024-12-11 20:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:639 on 2024-12-11 20:49_

yes

---

_Marked ready for review by @AlexWaygood on 2024-12-12 18:23_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-12 18:23_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-12 18:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-12 18:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:616 on 2024-12-12 18:29_

This seems like a tautology? I'm not sure what it's trying to say. I would expect it to say this instead:
```suggestion
        // Two equivalent types are always subtypes of each other.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:619 on 2024-12-12 18:30_

"can be determined to point to" feels like a roundabout and unnecessarily vague phrasing: for a fully static type there should never be any ambiguity about what set of objects it refers to
```suggestion
        // and name exactly the same set of possible runtime objects.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:633 on 2024-12-12 18:31_

```suggestion
        // But the set of objects named by a non fully static type is (either partially or wholly) unknown,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:634 on 2024-12-12 18:31_

```suggestion
        // so the question is simply unanswerable for non fully static types.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-12-12 20:01_

This works for all our existing `ClassLiteral` cases above, but in principle it's risky. Usually (e.g. with the `Literal` types above) when we delegate, we delegate to a _wider_ type, so the delegation can give a false negative if we missed a case for the narrower type, but it can't give a false positive answer.

In this case, we are delegating in the other direction, from a wider type `type[T]` to a narrower type `Literal[T]`, which in principle opens the door to a false positive.

I'm not seeing why we can't just flip this: write the cases above in terms of `Type::SubclassOf` instead of `Type::ClassLiteral`, and then have `ClassLiteral` delegate to `SubclassOf` instead. This seems safer and easier to reason about its correctness.

---

_@carljm approved on 2024-12-12 22:39_

Awesome, thank you!!

---

_@AlexWaygood reviewed on 2024-12-12 22:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:633 on 2024-12-12 22:47_

oops, thanks!

---

_@sharkdp reviewed on 2024-12-13 07:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:698 on 2024-12-13 07:51_

Not related to your PR, but it would look a bit more symmetric if we change the order here to what we have above for the subtype-of check for positive elements:
```suggestion
                        .all(|&neg_ty| self.is_disjoint_from(db, neg_ty))
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-12-13 08:07_

I just ran the property tests on this branch, and I think the following failure is related to this exact case:
```
thread 'types::property_tests::stable::subtype_of_is_transitive' panicked at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (BuiltinInstance("type"), SubclassOfBuiltinClass("object"), BuiltinClassLiteral("object"))
```

This means we currently model `type <: type[object]` and `type[object] <: Literal[object]`, but not `type <: Literal[object]`.

The problem seems to be that second subtype relation `type[object] <: Literal[object]`, which looks wrong. And seems to be handled by this exact match pattern.

---

_@sharkdp reviewed on 2024-12-13 08:07_

---

_@AlexWaygood reviewed on 2024-12-13 11:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:616 on 2024-12-13 11:21_

Thanks, sorry for the shoddy quality of the comments in this PR! I guess I started off with the goal of adding comments to make sure each branch was explicitly documented, but then got distracted by bugs in the semantics and forgot to do a second pass on the comments to make sure they made sense 🙃

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:619 on 2024-12-13 11:22_

```suggestion
        // and point to exactly the same set of possible runtime objects.
```

---

_@AlexWaygood reviewed on 2024-12-13 11:22_

---

_@AlexWaygood reviewed on 2024-12-13 11:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:633 on 2024-12-13 11:23_

```suggestion
        // But the set of objects described by a non-fully-static type is (either partially or wholly) unknown,
```

---

_@AlexWaygood reviewed on 2024-12-13 11:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:634 on 2024-12-13 11:23_

```suggestion
        // so the question is simply unanswerable for non-fully-static types.
```

---

_@AlexWaygood reviewed on 2024-12-13 11:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:619 on 2024-12-13 11:28_

I weakly prefer "described by" or "point to" rather than "name", because it feels slightly off to say that any type has a canonical "name". Perhaps you might say that any type can have multiple names, but that also feels confusing.

`type` (== `Instance("type")`) and `type[object]` (== `SubclassOf("object")`) both describe the same set of possible runtime objects, but I wouldn't really describe either as a more valid name than the other.

---

_@AlexWaygood reviewed on 2024-12-13 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-12-13 11:30_

Great observation from both of you, thank you! I confirmed that this test currently fails on my branch, when it should pass:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index aed779e53..6cd2139c0 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -3526,6 +3526,7 @@ pub(crate) mod tests {
     #[test_case(Ty::BuiltinInstance("type"), Ty::SubclassOfBuiltinClass("str"))]
     #[test_case(Ty::BuiltinClassLiteral("str"), Ty::SubclassOfAny)]
     #[test_case(Ty::AbcInstance("ABCMeta"), Ty::SubclassOfBuiltinClass("type"))]
+    #[test_case(Ty::SubclassOfBuiltinClass("str"), Ty::BuiltinClassLiteral("str"))]
     fn is_not_subtype_of(from: Ty, to: Ty) {
         let db = setup_db();
         assert!(!from.into_type(&db).is_subtype_of(&db, to.into_type(&db)));
```

---

_Comment by @AlexWaygood on 2024-12-13 12:02_

Thanks again! Would appreciate it if somebody could take another quick look before I land, since this is all quite subtle stuff

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-13 12:02_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-13 12:02_

---

_@carljm reviewed on 2024-12-13 14:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-12-13 14:24_

Aha, yes! When I reviewed the "above cases" to make sure they worked with this delegation, I of course forgot to consider the "is the same type" above case :) Chalk up another win for the property tests.

---

_@carljm reviewed on 2024-12-13 14:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:619 on 2024-12-13 14:30_

I don't agree that to "name" a set of objects implies "to uniquely name" a set of objects, or even "to preferentially name" a set of objects. A type can have multiple equally-valid names, and it is still perfectly accurate to say that any one of them "names" the type.

So I still (weakly :) ) prefer "name", and my second favorite options are "refers to" to or "describes". I find "points to" a bit awkward, personally. But I think any of them communicate well enough; as the author of the PR, you get to paint the bikeshed :)

---

_@AlexWaygood reviewed on 2024-12-13 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:619 on 2024-12-13 14:35_

I pushed a commit switching the verb to "describe", since it sounds like that's something we're both okay with ;)

---

_@carljm approved on 2024-12-13 14:37_

Looks good to me!

Did you re-run the property tests on the latest version of the PR?

---

_Comment by @AlexWaygood on 2024-12-13 19:19_

Okay, I had to add a couple of new branches to `is_disjoint_from` to make the property tests happy (we weren't recognising that `tuple[()]` was disjoint from `Literal[object]`, which was causing `is_subtype_from` not to be transitive w.r.t. intersections, since the `Type::Intersection` branch in `is_subtype_of` calls `is_disjoint_from` if there are negative elements in the intersection).

This PR now causes no new property-test failures relative to `main`! I tested using this invocation:

```
QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable
```

The `types::property_tests::stable::assignable_to_is_reflexive` still fails instantly on this branch, but it also fails on `main`, so I didn't look at that too much. All other property tests pass.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1033 on 2024-12-13 19:22_

Am I understanding right that this line (along with the similar ones removed below for `BytesLiteral` and `SliceLiteral`) were already redundant before this PR, because they were caught by the big union for single-valued types above?

---

_@carljm approved on 2024-12-13 19:23_

Looks good, thank you!

---

_@AlexWaygood reviewed on 2024-12-13 19:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1033 on 2024-12-13 19:24_

Exactly. And the compiler can now verify that this is the case, which led to me getting lots of warnings about unreachable branches locally.

---

_Merged by @AlexWaygood on 2024-12-13 19:31_

---

_Closed by @AlexWaygood on 2024-12-13 19:31_

---

_Branch deleted on 2024-12-13 19:31_

---
