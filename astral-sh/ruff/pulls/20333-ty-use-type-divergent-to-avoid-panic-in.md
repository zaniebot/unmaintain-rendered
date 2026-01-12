```yaml
number: 20333
title: "[ty] use Type::Divergent to avoid panic in infinitely-nested-tuple implicit attribute"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/tuplerec
created_at: 2025-09-10T15:00:14Z
updated_at: 2025-09-11T13:51:24Z
url: https://github.com/astral-sh/ruff/pull/20333
synced_at: 2026-01-12T15:56:59Z
```

# [ty] use Type::Divergent to avoid panic in infinitely-nested-tuple implicit attribute

---

_@carljm_

## Summary

Use `Type::Divergent` to avoid "too many iterations" panic on an infinitely-nested tuple in an implicit instance attribute.

The regression here is from checking all tuple elements to see if they contain a Divergent type. It's 5% on one project, 1% on another, and zero on the rest. I spent some time looking into eliminating this regression by tracking a flag on inference results to note if they could possibly contain any Divergent type, but this doesn't really work -- there are too many different ways a type containing a Divergent type could enter an inference result. Still thinking about whether there are other ways to reduce this. One option is if we see certain kinds of non-atomic types that are commonly expensive to check for Divergent, we could make `has_divergent_type` a Salsa query on those types.

## Test Plan

Added mdtest.

Co-authored-by: Alex Waygood <Alex.Waygood@Gmail.com>


---

_Label `ty` added by @carljm on 2025-09-10 15:00_

---

_Comment by @github-actions[bot] on 2025-09-10 15:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-11 13:14:44.923387518 +0000
+++ new-output.txt	2025-09-11 13:14:47.942419689 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12a7a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12a7a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-10 15:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~5MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-10 15:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Ftuplerec?runnerMode=WallTime)

### Merging #20333 will **degrade performances by 5.12%**

<sub>Comparing <code>cjm/tuplerec</code> (1f8073d) with <code>main</code> (59c8fda)</sub>



### Summary

`❌ 1` regression  
`✅ 7` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm%2Ftuplerec?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[static-frame] `` | 7.8 s | 8.2 s | -5.12% |


---

_Comment by @github-actions[bot] on 2025-09-10 15:26_

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

_Renamed from "Cjm/tuplerec" to "[ty] use Type::Divergent to avoid panic in infinitely-nested-tuple implicit attribute" by @carljm on 2025-09-10 16:01_

---

_Marked ready for review by @carljm on 2025-09-10 23:37_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-10 23:37_

---

_Review requested from @sharkdp by @carljm on 2025-09-10 23:37_

---

_Review requested from @dcreager by @carljm on 2025-09-10 23:37_

---

_@dcreager approved on 2025-09-11 01:40_

I like this new `Divergent` type

---

_@AlexWaygood reviewed on 2025-09-11 10:53_

All tests seem to pass if I apply this refactor to your branch (which gets rid of quite a few `.expect()` calls!) -- is there anything conceptually wrong with this?

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index df251700f8..1d53611bf6 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -387,19 +387,19 @@ impl<'db> InferenceRegion<'db> {
     }
 }
 
-#[derive(Debug, Clone, Copy, Eq, PartialEq, get_size2::GetSize)]
-enum CycleRecovery {
+#[derive(Debug, Clone, Copy, Eq, PartialEq, get_size2::GetSize, salsa::Update)]
+enum CycleRecovery<'db> {
     /// An initial-value for fixpoint iteration; all types are `Type::Never`.
     Initial,
     /// A divergence-fallback value for fixpoint iteration; all types are `Divergent`.
-    Divergent,
+    Divergent(ScopeId<'db>),
 }
 
-impl CycleRecovery {
-    fn merge(self, other: Option<CycleRecovery>) -> CycleRecovery {
+impl<'db> CycleRecovery<'db> {
+    fn merge(self, other: Option<CycleRecovery<'db>>) -> Self {
         if let Some(other) = other {
             match (self, other) {
-                (Self::Divergent, _) | (_, Self::Divergent) => Self::Divergent,
+                (Self::Divergent(scope), _) | (_, Self::Divergent(scope)) => Self::Divergent(scope),
                 _ => Self::Initial,
             }
         } else {
@@ -407,10 +407,10 @@ impl CycleRecovery {
         }
     }
 
-    fn fallback_type<'db>(self, scope_fn: impl FnOnce() -> ScopeId<'db>) -> Type<'db> {
+    fn fallback_type(self) -> Type<'db> {
         match self {
             Self::Initial => Type::Never,
-            Self::Divergent => Type::divergent(scope_fn()),
+            Self::Divergent(scope) => Type::divergent(scope),
         }
     }
 }
@@ -428,13 +428,10 @@ pub(crate) struct ScopeInference<'db> {
 #[derive(Debug, Eq, PartialEq, get_size2::GetSize, salsa::Update, Default)]
 struct ScopeInferenceExtra<'db> {
     /// Is this a cycle-recovery inference result, and if so, what kind?
-    cycle_recovery: Option<CycleRecovery>,
+    cycle_recovery: Option<CycleRecovery<'db>>,
 
     /// The diagnostics for this region.
     diagnostics: TypeCheckDiagnostics,
-
-    /// The scope for which this is an inference result, if we are a divergent cycle recovery.
-    scope: Option<ScopeId<'db>>,
 }
 
 impl<'db> ScopeInference<'db> {
@@ -470,12 +467,9 @@ impl<'db> ScopeInference<'db> {
     }
 
     fn fallback_type(&self) -> Option<Type<'db>> {
-        self.extra.as_ref().and_then(|extra| {
-            extra.cycle_recovery.map(|recovery| {
-                recovery
-                    .fallback_type(|| extra.scope.expect("Divergent inference should have scope"))
-            })
-        })
+        self.extra
+            .as_ref()
+            .and_then(|extra| extra.cycle_recovery.map(CycleRecovery::fallback_type))
     }
 }
 
@@ -509,7 +503,7 @@ pub(crate) struct DefinitionInference<'db> {
 #[derive(Debug, Eq, PartialEq, get_size2::GetSize, salsa::Update, Default)]
 struct DefinitionInferenceExtra<'db> {
     /// Is this a cycle-recovery inference result, and if so, what kind?
-    cycle_recovery: Option<CycleRecovery>,
+    cycle_recovery: Option<CycleRecovery<'db>>,
 
     /// The definitions that are deferred.
     deferred: Box<[Definition<'db>]>,
@@ -519,9 +513,6 @@ struct DefinitionInferenceExtra<'db> {
 
     /// For function definitions, the undecorated type of the function.
     undecorated_type: Option<Type<'db>>,
-
-    /// The scope this region is part of, if we are a divergent cycle recovery.
-    scope: Option<ScopeId<'db>>,
 }
 
 impl<'db> DefinitionInference<'db> {
@@ -605,12 +596,9 @@ impl<'db> DefinitionInference<'db> {
     }
 
     fn fallback_type(&self) -> Option<Type<'db>> {
-        self.extra.as_ref().and_then(|extra| {
-            extra.cycle_recovery.map(|recovery| {
-                recovery
-                    .fallback_type(|| extra.scope.expect("Divergent inference should have scope"))
-            })
-        })
+        self.extra
+            .as_ref()
+            .and_then(|extra| extra.cycle_recovery.map(CycleRecovery::fallback_type))
     }
 
     pub(crate) fn undecorated_type(&self) -> Option<Type<'db>> {
@@ -643,13 +631,10 @@ struct ExpressionInferenceExtra<'db> {
     diagnostics: TypeCheckDiagnostics,
 
     /// Is this a cycle recovery inference result, and if so, what kind?
-    cycle_recovery: Option<CycleRecovery>,
+    cycle_recovery: Option<CycleRecovery<'db>>,
 
     /// `true` if all places in this expression are definitely bound
     all_definitely_bound: bool,
-
-    /// The scope this region is part of (if we are a Divergent cycle recovery.)
-    scope: Option<ScopeId<'db>>,
 }
 
 impl<'db> ExpressionInference<'db> {
@@ -672,9 +657,8 @@ impl<'db> ExpressionInference<'db> {
         let _ = scope;
         Self {
             extra: Some(Box::new(ExpressionInferenceExtra {
-                cycle_recovery: Some(CycleRecovery::Divergent),
+                cycle_recovery: Some(CycleRecovery::Divergent(scope)),
                 all_definitely_bound: true,
-                scope: Some(scope),
                 ..ExpressionInferenceExtra::default()
             })),
             expressions: FxHashMap::default(),
@@ -700,12 +684,9 @@ impl<'db> ExpressionInference<'db> {
     }
 
     fn fallback_type(&self) -> Option<Type<'db>> {
-        self.extra.as_ref().and_then(|extra| {
-            extra.cycle_recovery.map(|recovery| {
-                recovery
-                    .fallback_type(|| extra.scope.expect("Divergent inference should have scope"))
-            })
-        })
+        self.extra
+            .as_ref()
+            .and_then(|extra| extra.cycle_recovery.map(CycleRecovery::fallback_type))
     }
 
     /// Returns true if all places in this expression are definitely bound.
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index b1e6fd3b4f..1264aa21fd 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -252,7 +252,7 @@ pub(super) struct TypeInferenceBuilder<'db, 'ast> {
     undecorated_type: Option<Type<'db>>,
 
     /// Did we merge in a sub-region with a cycle-recovery fallback, and if so, what kind?
-    cycle_recovery: Option<CycleRecovery>,
+    cycle_recovery: Option<CycleRecovery<'db>>,
 
     /// `true` if all places in this expression are definitely bound
     all_definitely_bound: bool,
@@ -293,7 +293,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         }
     }
 
-    fn extend_cycle_recovery(&mut self, other_recovery: Option<CycleRecovery>) {
+    fn extend_cycle_recovery(&mut self, other_recovery: Option<CycleRecovery<'db>>) {
         match &mut self.cycle_recovery {
             Some(recovery) => *recovery = recovery.merge(other_recovery),
             recovery @ None => *recovery = other_recovery,
@@ -301,8 +301,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
     }
 
     fn fallback_type(&self) -> Option<Type<'db>> {
-        self.cycle_recovery
-            .map(|recovery| recovery.fallback_type(|| self.scope))
+        self.cycle_recovery.map(CycleRecovery::fallback_type)
     }
 
     fn extend_definition(&mut self, inference: &DefinitionInference<'db>) {
@@ -8866,7 +8865,6 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                     diagnostics,
                     cycle_recovery,
                     all_definitely_bound,
-                    scope: Some(scope),
                 })
             });
 
@@ -8915,7 +8913,6 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 deferred: deferred.into_boxed_slice(),
                 diagnostics,
                 undecorated_type,
-                scope: Some(scope),
             })
         });
 
@@ -8981,7 +8978,6 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             Box::new(ScopeInferenceExtra {
                 cycle_recovery,
                 diagnostics,
-                scope: Some(scope),
             })
         });
```

---

_Comment by @carljm on 2025-09-11 13:06_

> All tests seem to pass if I apply this refactor to your branch... is there anything conceptually wrong with this?

There sort of is; I'd actually started the same refactor and then stopped when I reached the "conceptually wrong" thing, which is the implementation of `CycleRecovery::merge`. When we merge two `CycleRecovery::Divergent` together, with potentially different scopes, that should never change the scope of whichever one we're mutating. It makes `CycleRecovery::merge` not a symmetric operation.

That said, I _think_ your implementation actually achieves the necessary invariant in practice, but it's a bit subtle, and due solely to the way the clauses of the match pattern union are ordered, and the fact that `TypeInferenceBuilder::extend_cycle_recovery` always calls `CycleRecovery::merge` on `self.cycle_recovery`, not `other.cycle_recovery`. So it may still make sense to apply this change, just with a bit more explicit commentary about that being an important invariant.



---

_Merged by @carljm on 2025-09-11 13:51_

---

_Closed by @carljm on 2025-09-11 13:51_

---

_Branch deleted on 2025-09-11 13:51_

---
