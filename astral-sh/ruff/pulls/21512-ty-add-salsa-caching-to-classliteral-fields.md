```yaml
number: 21512
title: "[ty] Add Salsa caching to `ClassLiteral::fields`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-fields
created_at: 2025-11-18T15:29:43Z
updated_at: 2025-11-18T19:16:16Z
url: https://github.com/astral-sh/ruff/pull/21512
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Add Salsa caching to `ClassLiteral::fields`

---

_@AlexWaygood_

## Summary

Locally, applying this patch to @oconnor663's branch for https://github.com/astral-sh/ruff/pull/21467 speeds us up >120x on the pydantic benchmark (we go from taking a whole minute to complete the benchmark to taking <0.5s).

But I think this might provide a performance boost on `main`, which is why I'm filing it as a standalone PR.

## Test Plan

All existing tests pass


---

_Label `performance` added by @AlexWaygood on 2025-11-18 15:29_

---

_Label `ty` added by @AlexWaygood on 2025-11-18 15:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 15:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-18 17:45:25.535995063 +0000
+++ new-output.txt	2025-11-18 17:45:28.972019207 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:465:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19aa3)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:465:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19ea3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-18 15:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[]8;;https://ty.dev/rules#no-matching-overload\no-matching-overload]8;;\] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-18 15:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:2827 on 2025-11-18 15:38_

Seems reasonable to cache this. Can you add the `heap_size` attribute so that we can get a sense of how expensive this is in terms of memory consumption?

How much does it change performance if we were to cache `own_fields` only? I'm asking because that would require slightly less memory

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:2833 on 2025-11-18 15:41_

What's the reason why we need an `IndexMap` now?

---

_@MichaReiser reviewed on 2025-11-18 15:41_

---

_@AlexWaygood reviewed on 2025-11-18 15:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2827 on 2025-11-18 15:41_

> Can you add the `heap_size` attribute so that we can get a sense of how expensive this is in terms of memory consumption?

Yes, I was already working on this before you posted your comment 😆

> How much does it change performance if we were to cache `own_fields` only? I'm asking because that would require slightly less memory

Not sure. Happy to try it, but I'll wait for Codspeed to finish on this version of the PR first so we can compare the two using the codspeed runners

---

_@AlexWaygood reviewed on 2025-11-18 15:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2833 on 2025-11-18 15:41_

`salsa::Update` is implemented for `IndexMap` but not for `OrderMap`.

---

_@AlexWaygood reviewed on 2025-11-18 15:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2833 on 2025-11-18 15:42_

I can make a PR to Salsa to implement `salsa::Update` for `OrderMap` too if you'd prefer this method to continue returning `OrderMap`. But as far as I understand, we generally prefer to use `IndexMap`s where possible since `IndexMap` is a simpler type than `OrderMap` (`OrderMap` just adds `Hash` on top of `IndexMap`'s API, IIRC?). It doesn't seem necessary to use an `OrderMap` here.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:2833 on 2025-11-18 15:47_

Oh I see. Nah, I don't care it just wasn't obvious to me why we change it

---

_@MichaReiser reviewed on 2025-11-18 15:47_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 15:50_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @AlexWaygood on 2025-11-18 15:58_

[1-3% improvements on six benchmarks](https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-fields?utm_source=github&utm_medium=check&utm_content=details), including a 2% improvement on pydantic, offset by a 1% regression on 9 benchmarks. No memory usage change reported by mypy_primer.

---

_Comment by @MichaReiser on 2025-11-18 16:01_

Should we rebase this on top of Jack's PR to see the real performance benefits? It seems more questionable to merge with the now very limited performance improvement

---

_Comment by @carljm on 2025-11-18 16:22_

I think the options are "land this first" or "merge it with Jack's PR" -- I don't think we can land the TypedDict PR separately without this, because it makes benchmarks time out on GitHub.

I would be inclined to land this separately just because it's easier to review as two separate PRs. But we can rebase Jack's PR on top of this first to verify that this fixes the performance issues there.

---

_Comment by @AlexWaygood on 2025-11-18 16:47_

> Should we rebase this on top of Jack's PR to see the real performance benefits? It seems more questionable to merge with the now very limited performance improvement

Hmm, I'm not _totally_ sure I understand the standard being applied here -- I feel like we've merged lots of performance improvements in the past that showed <3% improvements on our benchmarks, especially if they didn't have any measurable memory-usage regressions :-)

As I say, I've also verified locally that this yields a _huge_ speedup when I cherry pick it onto #21467, which currently can't be merged due to the performance regression on that branch.

But sure, I'm happy if @oconnor663 wants to just cherry-pick https://github.com/astral-sh/ruff/pull/21512/commits/f4075371e05fe7d48a94e1c841b37e9a6f8e3dc3 onto #21467.

> How much does it change performance if we were to cache `own_fields` only? I'm asking because that would require slightly less memory

I tried this out locally (applied to Jack's PR). It has very competitive performance to this version. But I'm inclined to go with this version, since there's no memory regression reported on this PR, and since caching `ClassLiteral::own_fields` rather than `ClassLiteral::fields` makes our APIs a fair bit less ergonomic IMO. You end up always having to assign the result of `class.fields(db)` before iterating over it, or the borrow checker complains in several places that temporary values are being dropped while borrowed, because the return type of `ClassLiteral::fields()` needs to be `FxIndexMap<&'db Name, &'db Field<'db>>` rather than `&'db FxIndexMap<Name, Field<'db>>`

<details>

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index abc54f5626..54cfd75f9c 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2593,8 +2593,9 @@ impl<'db> ClassLiteral<'db> {
                 )))
             }
             (CodeGeneratorKind::TypedDict, "get") => {
-                let overloads = self
-                    .fields(db, specialization, field_policy)
+                let fields = self.fields(db, specialization, field_policy);
+
+                let overloads = fields
                     .iter()
                     .flat_map(|(name, field)| {
                         let key_type =
@@ -2824,17 +2825,19 @@ impl<'db> ClassLiteral<'db> {
     /// Returns a list of all annotated attributes defined in this class, or any of its superclasses.
     ///
     /// See [`ClassLiteral::own_fields`] for more details.
-    #[salsa::tracked(returns(ref))]
     pub(crate) fn fields(
         self,
         db: &'db dyn Db,
         specialization: Option<Specialization<'db>>,
         field_policy: CodeGeneratorKind<'db>,
-    ) -> FxIndexMap<Name, Field<'db>> {
+    ) -> FxIndexMap<&'db Name, &'db Field<'db>> {
         if field_policy == CodeGeneratorKind::NamedTuple {
             // NamedTuples do not allow multiple inheritance, so it is sufficient to enumerate the
             // fields of this class only.
-            return self.own_fields(db, specialization, field_policy);
+            return self
+                .own_fields(db, specialization, field_policy)
+                .iter()
+                .collect();
         }
 
         let matching_classes_in_mro: Vec<_> = self
@@ -2873,11 +2876,12 @@ impl<'db> ClassLiteral<'db> {
     ///     y: str = "a"
     /// ```
     /// we return a map `{"x": (int, None), "y": (str, Some(Literal["a"]))}`.
+    #[salsa::tracked(returns(ref))]
     pub(super) fn own_fields(
         self,
         db: &'db dyn Db,
         specialization: Option<Specialization<'db>>,
-        field_policy: CodeGeneratorKind,
+        field_policy: CodeGeneratorKind<'db>,
     ) -> FxIndexMap<Name, Field<'db>> {
         let mut attributes = FxIndexMap::default();
 
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index 66316f1537..5b3cc2b62e 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -3135,7 +3135,7 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
     typed_dict_ty: Type<'db>,
     full_object_ty: Option<Type<'db>>,
     key_ty: Type<'db>,
-    items: &FxIndexMap<Name, Field<'db>>,
+    items: &FxIndexMap<&'db Name, &'db Field<'db>>,
 ) {
     let db = context.db();
     if let Some(builder) = context.report_lint(&INVALID_KEY, key_node) {
@@ -3197,8 +3197,8 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
 pub(super) fn report_namedtuple_field_without_default_after_field_with_default<'db>(
     context: &InferContext<'db, '_>,
     class: ClassLiteral<'db>,
-    (field, field_def): &(Name, Option<Definition<'db>>),
-    (field_with_default, field_with_default_def): &(Name, Option<Definition<'db>>),
+    (field, field_def): (&Name, Option<Definition<'db>>),
+    (field_with_default, field_with_default_def): (&Name, Option<Definition<'db>>),
 ) {
     let db = context.db();
     let module = context.module();
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index caa925ad74..778fe849b2 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -4,6 +4,7 @@ use itertools::{Either, Itertools};
 use ruff_db::diagnostic::{Annotation, DiagnosticId, Severity};
 use ruff_db::files::File;
 use ruff_db::parsed::ParsedModuleRef;
+use ruff_python_ast::name::Name;
 use ruff_python_ast::visitor::{Visitor, walk_expr};
 use ruff_python_ast::{
     self as ast, AnyNodeRef, ExprContext, HasNodeIndex, NodeIndex, PythonVersion,
@@ -613,12 +614,11 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                     ) {
                         field_with_default_encountered =
                             Some((field_name, field.single_declaration));
-                    } else if let Some(field_with_default) = field_with_default_encountered.as_ref()
-                    {
+                    } else if let Some(field_with_default) = field_with_default_encountered {
                         report_namedtuple_field_without_default_after_field_with_default(
                             &self.context,
                             class,
-                            &(field_name, field.single_declaration),
+                            (field_name, field.single_declaration),
                             field_with_default,
                         );
                     }
@@ -919,9 +919,9 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 CodeGeneratorKind::from_class(self.db(), class, None)
             {
                 let specialization = None;
+                let fields = class.fields(self.db(), specialization, field_policy);
 
-                let kw_only_sentinel_fields: Vec<_> = class
-                    .fields(self.db(), specialization, field_policy)
+                let kw_only_sentinel_fields: Vec<_> = fields
                     .iter()
                     .filter_map(|(name, field)| {
                         field.is_kw_only_sentinel(self.db()).then_some(name)
@@ -7269,7 +7269,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             }
 
             let value_ty = if let Some(Type::StringLiteral(key)) = key_ty
-                && let Some(field) = typed_dict_items.get(key.value(self.db()))
+                && let Some(field) = typed_dict_items.get(&Name::from(key.value(self.db())))
             {
                 self.infer_expression(&item.value, TypeContext::new(Some(field.declared_ty)))
             } else {
@@ -7954,7 +7954,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                                     Type::TypedDict(typed_dict_ty),
                                     None,
                                     key_ty,
-                                    items,
+                                    &items,
                                 );
                                 // Return `Unknown` to prevent the overload system from generating its own error
                                 return Type::unknown();
@@ -11285,7 +11285,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                                 value_ty,
                                 None,
                                 slice_ty,
-                                typed_dict.items(db),
+                                &typed_dict.items(db),
                             );
                         } else {
                             if let Some(builder) =
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 1b33d7111f..521329f2c9 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -58,7 +58,7 @@ impl<'db> TypedDictType<'db> {
         self.defining_class
     }
 
-    pub(crate) fn items(self, db: &'db dyn Db) -> &'db FxIndexMap<Name, Field<'db>> {
+    pub(crate) fn items(self, db: &'db dyn Db) -> FxIndexMap<&'db Name, &'db Field<'db>> {
         let (class_literal, specialization) = self.defining_class.class_literal(db);
         class_literal.fields(db, specialization, CodeGeneratorKind::TypedDict)
     }
@@ -318,7 +318,7 @@ pub(super) fn validate_typed_dict_key_assignment<'db, 'ast>(
     let items = typed_dict.items(db);
 
     // Check if key exists in `TypedDict`
-    let Some((_, item)) = items.iter().find(|(name, _)| *name == key) else {
+    let Some((_, item)) = items.iter().find(|(name, _)| **name == key) else {
         if emit_diagnostic {
             report_invalid_key_on_typed_dict(
                 context,
@@ -327,7 +327,7 @@ pub(super) fn validate_typed_dict_key_assignment<'db, 'ast>(
                 Type::TypedDict(typed_dict),
                 full_object_ty,
                 Type::string_literal(db, key),
-                items,
+                &items,
             );
         }
```

</details>

---

_Marked ready for review by @AlexWaygood on 2025-11-18 16:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-18 16:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-18 16:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-18 16:53_

---

_Review comment by @carljm on `crates/ruff_memory_usage/src/lib.rs`:36 on 2025-11-18 17:05_

Is there a reference you used to verify the actual memory use of an IndexMap that could be linked here?

---

_@carljm approved on 2025-11-18 17:07_

Thank you!

---

_@AlexWaygood reviewed on 2025-11-18 17:10_

---

_Review comment by @AlexWaygood on `crates/ruff_memory_usage/src/lib.rs`:36 on 2025-11-18 17:10_

no, this is just the `OrderMap` implementation a few lines above directly copied and pasted. But (assuming the pre-existing `OrderMap` implementation is correct), I think that that should be correct. An `OrderMap` is [just a thin wrapper around an `IndexMap`](https://github.com/indexmap-rs/ordermap/blob/d04e267f4cd2533b6e83e98e083942becf29c778/src/map.rs#L102-L105)

---

_Review comment by @ibraheemdev on `crates/ruff_memory_usage/src/lib.rs`:36 on 2025-11-18 17:23_

get-size2 already has support for `IndexMap`, so I'm not sure why we need this. Maybe the `indexmap` feature is not enabled somewhere?

---

_@ibraheemdev reviewed on 2025-11-18 17:23_

---

_@MichaReiser approved on 2025-11-18 17:40_

---

_@AlexWaygood reviewed on 2025-11-18 17:44_

---

_Review comment by @AlexWaygood on `crates/ruff_memory_usage/src/lib.rs`:36 on 2025-11-18 17:44_

Great catch -- yes, that was it! Fixed.

---

_Merged by @AlexWaygood on 2025-11-18 17:48_

---

_Closed by @AlexWaygood on 2025-11-18 17:48_

---

_Branch deleted on 2025-11-18 17:48_

---

_Comment by @oconnor663 on 2025-11-18 19:16_

Thank you @AlexWaygood (and reviewers)!

---
