```yaml
number: 14109
title: "[red-knot] Precise inference for identity checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/identity-check-inference
created_at: 2024-11-05T13:45:09Z
updated_at: 2024-11-05T18:48:54Z
url: https://github.com/astral-sh/ruff/pull/14109
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Precise inference for identity checks

---

_Pull request opened by @sharkdp on 2024-11-05 13:45_

## Summary

Adds more precise type inference for `… is …` and `… is not …` identity checks in some limited cases where we statically know the answer to be either `Literal[True]` or `Literal[False]`.

I found this helpful while working on type inference for comparisons involving intersection types, but I'm not sure if this is at all useful for real world code (where the answer is most probably *not* statically known). Note that we already have *type narrowing* for identity tests. So while we are already able to generate constraints for things like `if x is None`, we can now — in some limited cases — make an even stronger conclusion and infer that the test expression itself is `Literal[False]` (branch never taken) or `Literal[True]` (branch always taken).

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2024-11-05 13:45_

---

_Review requested from @carljm by @sharkdp on 2024-11-05 13:45_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-05 13:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-05 13:45_

---

_@sharkdp reviewed on 2024-11-05 13:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3374 on 2024-11-05 13:46_

The `reveal_type(a1 is o) # revealed: bool` test (and similar) make sure that we don't forget about this.

---

_Comment by @github-actions[bot] on 2024-11-05 13:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3378 on 2024-11-05 15:13_

Shouldn't this be

```suggestion
                            if left.is_singleton(self.db)
```

? And same for the `CmpOp::IsNot` branch below. It's hard to think of a test that would fail because of this issue, because at this point we know that both `left` and `right` are `InstanceType`s -- I can't think of any `InstanceType`s that would be single-valued types without being singleton types. But I think using `is_singleton` would be more _correct_ and less confusing here, per the definitions we're adding in https://github.com/astral-sh/ruff/pull/13878. (I need to merge that PR!)

---

_@AlexWaygood reviewed on 2024-11-05 15:13_

---

_@sharkdp reviewed on 2024-11-05 15:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3378 on 2024-11-05 15:24_

Yes, of course :see_no_evil:.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3395 on 2024-11-05 15:38_

You can simplify this further by utilising the fact that we know at this point that both `left` and `right` are `InstanceType`s:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index c93e44f5f..e3e6053b5 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -3374,27 +3374,19 @@ impl<'db> TypeInferenceBuilder<'db> {
                         // positive answers for narrowing the type.
                         if left.is_disjoint_from(self.db, right) {
                             Ok(Type::BooleanLiteral(false))
+                        } else if left.is_singleton(self.db) && left_class == right_class {
+                            Ok(Type::BooleanLiteral(true))
                         } else {
-                            if left.is_single_valued(self.db)
-                                && left.is_equivalent_to(self.db, right)
-                            {
-                                Ok(Type::BooleanLiteral(true))
-                            } else {
-                                Ok(KnownClass::Bool.to_instance(self.db))
-                            }
+                            Ok(KnownClass::Bool.to_instance(self.db))
                         }
                     }
                     ast::CmpOp::IsNot => {
                         if left.is_disjoint_from(self.db, right) {
                             Ok(Type::BooleanLiteral(true))
+                        } else if left.is_singleton(self.db) && left_class == right_class {
+                            Ok(Type::BooleanLiteral(false))
                         } else {
-                            if left.is_single_valued(self.db)
-                                && left.is_equivalent_to(self.db, right)
-                            {
-                                Ok(Type::BooleanLiteral(false))
-                            } else {
-                                Ok(KnownClass::Bool.to_instance(self.db))
-                            }
+                            Ok(KnownClass::Bool.to_instance(self.db))
                         }
                     }
                 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 15:44_

Could you also add a test for an `InstanceType` that inherits from an `Unknown` class?

E.g. something like this:

```py
from does_not_exist import this_will_be_inferred_as_Unknown

class Gradual(this_will_be_inferred_as_Unknown): ...
class FullyStatic: ...

x = Gradual()
y = FullyStatic()

reveal_type(x is y)  # revealed: bool
```

I think it's important that this reveals `bool` rather than `Literal[True]` or `Literal[False]`, because we don't know the "true MRO" of `Gradual`, since it inherits from `Unknown`. All of this should already work as expected with your PR branch, I think; I'd just like this test to make sure that it doesn't regress in the future.

---

_@AlexWaygood approved on 2024-11-05 15:44_

Nice!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3374 on 2024-11-05 16:34_

I'm not sure the premise of this comment is quite correct? Even if `is_equivalent_to` were perfect (never returned a false negative), we could not replace `if left.is_disjoint_from(self.db, right)` with `if !left.is_equivalent_to(self.db, right)` in the code below.

If the types of `left` and `right` are disjoint, no object can be in both types, so the `is` check must be false. But if `left` and `right` are simply not-equivalent, it's still possible that they overlap, and thus we don't have any additional information about the result of the `is` check.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3395 on 2024-11-05 16:40_

I'm not sure we should reduce `is_equivalent_to` to `left_class == right_class`, even if we know both sides are instances. That would mean we wouldn't consider an instance of `types.NoneType` equivalent to `_typeshed.NoneType`. Maybe that's OK in practice once we have version-info checks, but I still think it's safer to keep our "are these two types equivalent" logic in a single place.

This comment does make me wonder how many of the above special cases for non-instance types could be boiled down to this same generic logic using disjointness, singleton-ness, and equivalence. But probably not worth pursuing that since the existing special-cases work, and exploiting this would require restructuring the entire function.

---

_@carljm approved on 2024-11-05 16:40_

---

_@sharkdp reviewed on 2024-11-05 16:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 16:43_

Hm.. but we use neither subclass, subtype nor assign-ability semantics for this. We use non-disjointness (or ideally, exact equivalence of types) to make statements about these identity checks.

Whether or not `A` is a subclass of `B` doesn't help us with making any statements about `a is b` tests. If it is a subclass, we certainly can't decide if `a is b` is true or false. And if `A` is not a subclass, it could be a superclass. Which is the exact same situation, just with `A` and `B` reversed.

What I'm saying is: Sure, I can add this test. But I don't really see a realistic scenario where it would fail? Like, how would you make this particular test fail while maintaining successes in all other test cases? Even for arbitrary "fully static"/not-`Unknown` classes, we would infer `bool` here, since they are not disjoint.

---

_@carljm reviewed on 2024-11-05 16:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 16:44_

I'm not sure how important it is to add this test for the particular case of `is` comparisons, since the inheritance-from-Unknown is irrelevant here -- even if `x` and `y` are both instances of fully-static types, we could only conclude `bool` for `x is y`, and that is already tested here.

If one of them were a final type, that would change.

Ultimately I think this kind of subtlety is more a test of our `is_disjoint_from` logic, and it's probably better to push the tests down to that level, rather than trying to test these kinds of cases separately for every type inference feature that uses disjointness?

---

_@AlexWaygood reviewed on 2024-11-05 16:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 16:47_

We can leave the test. I was mainly just trying to think of edge cases, and inheritance from `Unknown` always makes me pause for thought 😄 I agree that the main thing that this would be testing would be our `is_disjoint_from` implementation -- though I'd still prefer to have mdtests where possible as well as unit tests, since there's always a risk that we might remove that method in the future in some refactor, and I'd want confidence that our inference would stay the same in that eventuality.

---

_@AlexWaygood reviewed on 2024-11-05 16:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3395 on 2024-11-05 16:49_

> That would mean we wouldn't consider an instance of `types.NoneType` equivalent to `_typeshed.NoneType`. Maybe that's OK in practice once we have version-info checks, but I still think it's safer to keep our "are these two types equivalent" logic in a single place.

I find that example pretty unconvincing. Whether we have multi-version checking or single-version checking, it's essential that we're able to infer that an object can either be of type `types.NoneType` or `_typeshed.NoneType`, but that the two types can never exist simultaneously on any version of Python.

---

_@carljm reviewed on 2024-11-05 17:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3395 on 2024-11-05 17:03_

I think you're right that the two types should never co-exist, as long as (in a multi-version-checking future) we ensure that the union we would get from `_typeshed.NoneType` simplifies to `types.NoneType`, so that if someone explicitly imports and uses `types.NoneType`, it still matches our inference for `None`.

But I don't think the specific example is so important. I think in general it is safer that our type algebra logic remain consolidated on `Type` methods, rather than spreading implicit assumptions about invariants throughout the code. I don't think there is enough value to be gained from "inlining" the definition of `Type::is_equivalent_to` for instance types here, to be worth the constraint that we must now ensure that the definition of `is_equivalent_to` for instance types always remains equality on the wrapped instances. (And that isn't even true today!)

---

_@AlexWaygood reviewed on 2024-11-05 17:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3395 on 2024-11-05 17:36_

It doesn't seem that implicit to me! But I don't feel strongly 😆 happy to leave it as-is if you prefer it that way :-)

---

_@sharkdp reviewed on 2024-11-05 18:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3374 on 2024-11-05 18:18_

You are right. My flawed thinking was: if two objects are *identical*, their types would also be the same. But that's not true of course. We can refer to the same exact object using more or less precise types.

---

_@sharkdp reviewed on 2024-11-05 18:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 18:27_

I still don't understand how it relates to this particular changeset, but I will write down a task to look into `is_disjoint_from` (unit) tests for `Type::Unknown`.

---

_@carljm reviewed on 2024-11-05 18:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 18:39_

I don't think it does relate to this changeset, and I don't think you need to note down a task particularly for it, either. Even for `is_disjoint_from` between instances, the case of inheriting Unknown will only become relevant or testable once we add support for final classes, and I think we can just include tests for it at that point.

---

_@carljm approved on 2024-11-05 18:39_

---

_@AlexWaygood reviewed on 2024-11-05 18:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md`:1 on 2024-11-05 18:44_

Yeah, I wouldn't worry about it. I was just trying to think of edge cases. It's definitely not something we need to prioritise :-)

---

_Merged by @sharkdp on 2024-11-05 18:48_

---

_Closed by @sharkdp on 2024-11-05 18:48_

---

_Branch deleted on 2024-11-05 18:48_

---
