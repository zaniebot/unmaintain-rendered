```yaml
number: 15396
title: "[red-knot] Move intersection type tests to Markdown"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/intersection-type-tests
created_at: 2025-01-10T11:01:24Z
updated_at: 2025-01-10T21:37:06Z
url: https://github.com/astral-sh/ruff/pull/15396
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Move intersection type tests to Markdown

---

_Pull request opened by @sharkdp on 2025-01-10 11:01_

## Summary

[**Rendered version of the new test suite**](https://github.com/astral-sh/ruff/blob/david/intersection-type-tests/crates/red_knot_python_semantic/resources/mdtest/intersection_types.md)

Moves most of our existing intersection-types tests to a dedicated Markdown test suite, extends the test coverage, unifies the notation for these tests, groups tests into a proper structure, and adds some explanations for various simplification strategies.

This changeset also:
  - Adds a new simplification where `~Never` is removed from intersections.
  - Adds a new simplification where adding `~object` simplifies the whole intersection to `Never`
  - Avoids unnecessary assignment-checks between inferred and declared type. This was added to this changeset to avoid many false positive errors in this test suite.

Resolves the task described in this old comment [here](https://github.com/astral-sh/ruff/pull/13962/files/e01da82a5a0ef6a2af0aa4dc50f898cffadb4a33..e7e432bca2b3f24979da55a9a34ad765aaaae8d1#r1819924085).

## Test Plan

Running the new Markdown tests


---

_Label `red-knot` added by @sharkdp on 2025-01-10 11:01_

---

_Review requested from @carljm by @sharkdp on 2025-01-10 11:01_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-10 11:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-10 11:01_

---

_Comment by @github-actions[bot] on 2025-01-10 11:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:7 on 2025-01-10 11:35_

```suggestion
intersection types (note that we display negative contributions at the end; the order does not matter):
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:34 on 2025-01-10 11:36_

```suggestion
We use `P`, `Q`, `R`, … to denote types that are non-disjoint:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:48 on 2025-01-10 11:38_

Because I keep forgetting why this is the case:

```suggestion

Although `P` is not a subtype of `Q` and `Q` is not a subtype of `P`, the two types are not disjoint because it would be possible to create a class `S` that inherits from both `P` and `Q` using multiple inheritance. An instance of `S` would be a member of the `P` type _and_ the `Q` type.

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:103 on 2025-01-10 11:42_

```suggestion
### Single-element unions
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:192 on 2025-01-10 11:43_

```suggestion
We always normalize our representation to a *union of intersections*, so when we add a *union to an
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:196 on 2025-01-10 11:45_

probably off-topic for this PR, but: I think we should add parentheses to the display for these. I find it impossible to remember what the precedence is, and I think users will as well. This would be much easier to read IMO:

```suggestion
    reveal_type(i1)  # revealed: (P & Q) | (P & R) | (P & S)
    reveal_type(i2)  # revealed: (P & S) | (Q & S) | (R & S)
    reveal_type(i3)  # revealed: (P & R) | (Q & R) | (P & S) | (Q & S)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 11:51_

Just to make sure I understand properly: this follows from the intuition that the type `~int` is really a shorthand way of saying `object & ~int`, and therefore `~Never` is a shorthand for `object & ~Never`, which is the same as `<the set of all possible objects> - <the empty set>`, which is the same as `<the set of all possible objects>`, which is the `object` type. Right?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:388 on 2025-01-10 11:53_

Following https://github.com/astral-sh/ruff/pull/15272, we should now understand e.g. `type[bool]` and `type[str]` as disjoint -- since `bool` is an `@final` class, we should eagerly simplify the type `type[bool]` to `Literal[bool]`, which we should understand as being disjoint from `type[str]`. It could be worth adding a test for that here?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:426 on 2025-01-10 11:53_

Very elegantly described :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:650 on 2025-01-10 11:55_

reminds me of the "he protecc" meme 😆 most importantly -- he intersecc

```suggestion
Dynamic types do not cancel each other out. Intersecting an unknown set of values with the negation
```

---

_@AlexWaygood reviewed on 2025-01-10 11:58_

The tests look fantastic! Haven't reviewed the code changes yet

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:314 on 2025-01-10 11:59_

may as well? it could be handy in tests at some point

```suggestion
#[derive(Debug, Clone, PartialEq, Eq)]
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:196 on 2025-01-10 12:03_

I think I personally prefer to remove the parentheses, so let's indeed leave it for another day :smile: 

---

_@sharkdp reviewed on 2025-01-10 12:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:319 on 2025-01-10 12:11_

Worth using a struct variant for this? Feel like it would remove the chance of us forgetting which is which

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -316,7 +316,10 @@ enum DeclaredAndInferredType<'db> {
     /// We know that both the declared and inferred types are the same.
     AreTheSame(Type<'db>),
     /// Declared and inferred types might be different, we need to check assignability.
-    MightBeDifferent(Type<'db>, Type<'db>),
+    MightBeDifferent {
+        declared: Type<'db>,
+        inferred: Type<'db>,
+    },
 }
 
 /// Builder to infer all types in a region.
@@ -911,13 +914,13 @@ impl<'db> TypeInferenceBuilder<'db> {
 
         let (declared_ty, inferred_ty) = match declared_and_inferred_ty {
             DeclaredAndInferredType::AreTheSame(ty) => (ty, ty),
-            DeclaredAndInferredType::MightBeDifferent(declared_ty, inferred_ty) => {
-                if inferred_ty.is_assignable_to(self.db(), *declared_ty) {
-                    (declared_ty, inferred_ty)
+            DeclaredAndInferredType::MightBeDifferent { declared, inferred } => {
+                if inferred.is_assignable_to(self.db(), *declared) {
+                    (declared, inferred)
                 } else {
-                    report_invalid_assignment(&self.context, node, *declared_ty, *inferred_ty);
+                    report_invalid_assignment(&self.context, node, *declared, *inferred);
                     // if the assignment is invalid, fall back to assuming the annotation is correct
-                    (declared_ty, declared_ty)
+                    (declared, declared)
                 }
             }
         };
@@ -1212,10 +1215,10 @@ impl<'db> TypeInferenceBuilder<'db> {
             let declared_ty = self.file_expression_ty(annotation);
             let declared_and_inferred_ty = if let Some(default_ty) = default_ty {
                 if default_ty.is_assignable_to(self.db(), declared_ty) {
-                    DeclaredAndInferredType::MightBeDifferent(
-                        declared_ty,
-                        UnionType::from_elements(self.db(), [declared_ty, default_ty]),
-                    )
+                    DeclaredAndInferredType::MightBeDifferent {
+                        declared: declared_ty,
+                        inferred: UnionType::from_elements(self.db(), [declared_ty, default_ty]),
+                    }
                 } else if self.in_stub()
                     && default
                         .as_ref()
@@ -2083,7 +2086,10 @@ impl<'db> TypeInferenceBuilder<'db> {
             self.add_declaration_with_binding(
                 assignment.into(),
                 definition,
-                &DeclaredAndInferredType::MightBeDifferent(annotation_ty, value_ty),
+                &DeclaredAndInferredType::MightBeDifferent {
+                    declared: annotation_ty,
+                    inferred: value_ty,
+                },
             );
```

---

_@AlexWaygood approved on 2025-01-10 12:13_

Nice. The code changes feel like they're kinda papering over the underlying issue a _little_ bit (we really _should_ recognise `int & Any` as being assignable to `int & Any`!!), but I think skipping the assignability check for function-parameter definitions is probably a worthwhile optimisation anyway. So this LGTM

---

_@sharkdp reviewed on 2025-01-10 12:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 12:15_

Yes, exactly. In this document, I tried to move away a bit from how we represent these types internally. I think it's not important that we represent `~T` as an intersection with a single negative contribution (where the empty "positive side" represents `object`). What's more important is that the negation-operation represents the set-theoretic complement w.r.t. the set of all possible values (`object`).

Set theory calls this set of all possible elements the "universe". In our case, `object` is the universe. And `Never` is the empty set ∅. The tests here are a manifestation of the universal complement laws

U<sup>∁</sup> = ∅
∅<sup>∁</sup> = U

I wonder if it makes sense to refer to these more set-theoretic terms in the document. And if it makes sense to test a few more of the [laws](https://en.wikipedia.org/wiki/Complement_(set_theory)#Properties_2) in a systematic way (I think we probably test most of them already, but it's distributed across the whole document).

---

_@sharkdp reviewed on 2025-01-10 12:17_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:319 on 2025-01-10 12:17_

Ah, yes. It was named `DeclaredAndInferred` at first, but then I changed the name of the variant.

---

_@sharkdp reviewed on 2025-01-10 12:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 12:53_

That actually revealed one more thing that we *could* simplify. I chose not to implement this right now, as it seems rather costly: if we see `T | ~T`, we could simplify that to `object`. I added a test that describes that we don't perform this optimization at the moment.

---

_@AlexWaygood approved on 2025-01-10 12:55_

Excellent!

---

_@sharkdp reviewed on 2025-01-10 12:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 12:56_

Let me know if you think that we should add this. I think it would be straightforward to add a (simple) version of this.

---

_@AlexWaygood reviewed on 2025-01-10 12:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 12:59_

> Let me know if you think that we should add this.

The simplification of `T | ~T` to `object`? Yes, I think we probably should simplify that. But it can probably be done as a followup, right? This PR looks mergeable right now to me

---

_Merged by @sharkdp on 2025-01-10 13:04_

---

_Closed by @sharkdp on 2025-01-10 13:04_

---

_Branch deleted on 2025-01-10 13:04_

---

_@sharkdp reviewed on 2025-01-10 13:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:321 on 2025-01-10 13:34_

Working on this

---

_@carljm reviewed on 2025-01-10 16:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:110 on 2025-01-10 16:20_

This should say "intersection of a single element"?

---

_@carljm reviewed on 2025-01-10 16:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:518 on 2025-01-10 16:26_

"Here we can get remove" -> "Here we can remove"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:426 on 2025-01-10 16:27_

"overlaps" seems not strong enough here, since it only means "not disjoint from" -- what is needed for this simplification to be valid is recognizing that ~Y is necessarily a super-set of X, if X and Y are disjoint.

---

_@carljm reviewed on 2025-01-10 16:28_

---

_@dcreager reviewed on 2025-01-10 16:51_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:48 on 2025-01-10 16:51_

Gah yes of course!  I was thinking exactly this, thank you for the clarification

---

_@sharkdp reviewed on 2025-01-10 21:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:426 on 2025-01-10 21:37_

Yes. I was inadvertently using "overlaps with" to mean "fully overlaps with" (pictorially speaking), I'll fix this.

---
