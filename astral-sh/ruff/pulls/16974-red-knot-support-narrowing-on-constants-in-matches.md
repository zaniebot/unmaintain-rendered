```yaml
number: 16974
title: "[red-knot] support narrowing on constants in matches"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: match-value-narrowing
created_at: 2025-03-26T04:35:23Z
updated_at: 2025-03-28T04:53:38Z
url: https://github.com/astral-sh/ruff/pull/16974
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] support narrowing on constants in matches

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of #13694

The implementation here was suspiciously straightforward so please lmk if I missed something

Also some drive-by changes to DRY things up a bit

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Add new tests to narrow/match.md


---

_Review requested from @carljm by @ericmarkmartin on 2025-03-26 04:35_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-03-26 04:35_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-03-26 04:35_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-03-26 04:35_

---

_Comment by @github-actions[bot] on 2025-03-26 04:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-03-26 11:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:485 on 2025-03-26 17:21_

this is nice, though it's a shame that we can't use it in the `evaluate_expr_name` method and in a few other methods. Maybe we should do something like this instead?

```diff
diff --git a/crates/red_knot_python_semantic/src/types/narrow.rs b/crates/red_knot_python_semantic/src/types/narrow.rs
index 00c270eba..fe7eddd0b 100644
--- a/crates/red_knot_python_semantic/src/types/narrow.rs
+++ b/crates/red_knot_python_semantic/src/types/narrow.rs
@@ -263,11 +263,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         is_positive: bool,
     ) -> NarrowingConstraints<'db> {
         let ast::ExprName { id, .. } = expr_name;
-
-        let symbol = self
-            .symbols()
-            .symbol_id_by_name(id)
-            .expect("Should always have a symbol for every Name node");
+        let symbol = self.expect_expr_name_symbol(id);
         let mut constraints = NarrowingConstraints::default();
 
         constraints.insert(
@@ -338,10 +334,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
                     id,
                     ctx: _,
                 }) => {
-                    let symbol = self
-                        .symbols()
-                        .symbol_id_by_name(id)
-                        .expect("Should always have a symbol for every Name node");
+                    let symbol = self.expect_expr_name_symbol(id);
 
                     match if is_positive { *op } else { op.negate() } {
                         ast::CmpOp::IsNot => {
@@ -408,10 +401,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
                         .into_class_literal()
                         .is_some_and(|c| c.class().is_known(self.db, KnownClass::Type))
                     {
-                        let symbol = self
-                            .symbols()
-                            .symbol_id_by_name(id)
-                            .expect("Should always have a symbol for every Name node");
+                        let symbol = self.expect_expr_name_symbol(id);
                         constraints.insert(symbol, Type::instance(rhs_class));
                     }
                 }
@@ -474,14 +464,11 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         }
     }
 
-    fn get_subject_symbol(&self, subject: Expression<'db>) -> Option<ScopedSymbolId> {
-        let ast::ExprName { id, .. } = subject.node_ref(self.db).as_name_expr()?;
-        let symbol = self
-            .symbols()
-            .symbol_id_by_name(id)
-            .expect("We should always have a symbol for every `Name` node");
-
-        Some(symbol)
+    #[track_caller]
+    fn expect_expr_name_symbol(&self, symbol: &str) -> ScopedSymbolId {
+        self.symbols()
+            .symbol_id_by_name(symbol)
+            .expect("We should always have a symbol for every `Name` node")
     }
 
     fn evaluate_match_pattern_singleton(
@@ -489,7 +476,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         subject: Expression<'db>,
         singleton: ast::Singleton,
     ) -> Option<NarrowingConstraints<'db>> {
-        let symbol = self.get_subject_symbol(subject)?;
+        let symbol = self.expect_expr_name_symbol(&subject.node_ref(self.db).as_name_expr()?.id);
 
         let ty = match singleton {
             ast::Singleton::None => Type::none(self.db),
@@ -506,7 +493,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         subject: Expression<'db>,
         cls: Expression<'db>,
     ) -> Option<NarrowingConstraints<'db>> {
-        let symbol = self.get_subject_symbol(subject)?;
+        let symbol = self.expect_expr_name_symbol(&subject.node_ref(self.db).as_name_expr()?.id);
         let ty = infer_same_file_expression_type(self.db, cls).to_instance(self.db)?;
 
         let mut constraints = NarrowingConstraints::default();
@@ -519,7 +506,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         subject: Expression<'db>,
         value: Expression<'db>,
     ) -> Option<NarrowingConstraints<'db>> {
-        let symbol = self.get_subject_symbol(subject)?;
+        let symbol = self.expect_expr_name_symbol(&subject.node_ref(self.db).as_name_expr()?.id);
         let ty = infer_same_file_expression_type(self.db, value);
         let mut constraints = NarrowingConstraints::default();
         constraints.insert(symbol, ty);
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:527 on 2025-03-26 17:26_

it looks to me like we could probably simplify this pattern throughout this file:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/narrow.rs b/crates/red_knot_python_semantic/src/types/narrow.rs
index 00c270eba..f20974cb9 100644
--- a/crates/red_knot_python_semantic/src/types/narrow.rs
+++ b/crates/red_knot_python_semantic/src/types/narrow.rs
@@ -268,18 +268,15 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
             .symbols()
             .symbol_id_by_name(id)
             .expect("Should always have a symbol for every Name node");
-        let mut constraints = NarrowingConstraints::default();
 
-        constraints.insert(
+        NarrowingConstraints::from_iter([(
             symbol,
             if is_positive {
                 Type::AlwaysFalsy.negate(self.db)
             } else {
                 Type::AlwaysTruthy.negate(self.db)
             },
-        );
-
-        constraints
+        )])
     }
 
     fn evaluate_expr_compare(
@@ -453,9 +450,10 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
                 function
                     .generate_constraint(self.db, class_info_ty)
                     .map(|constraint| {
-                        let mut constraints = NarrowingConstraints::default();
-                        constraints.insert(symbol, constraint.negate_if(self.db, !is_positive));
-                        constraints
+                        NarrowingConstraints::from_iter([(
+                            symbol,
+                            constraint.negate_if(self.db, !is_positive),
+                        )])
                     })
             }
             // for the expression `bool(E)`, we further narrow the type based on `E`
@@ -496,9 +494,8 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
             ast::Singleton::True => Type::BooleanLiteral(true),
             ast::Singleton::False => Type::BooleanLiteral(false),
         };
-        let mut constraints = NarrowingConstraints::default();
-        constraints.insert(symbol, ty);
-        Some(constraints)
+
+        Some(NarrowingConstraints::from_iter([(symbol, ty)]))
     }
 
     fn evaluate_match_pattern_class(
@@ -509,9 +506,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
         let symbol = self.get_subject_symbol(subject)?;
         let ty = infer_same_file_expression_type(self.db, cls).to_instance(self.db)?;
 
-        let mut constraints = NarrowingConstraints::default();
-        constraints.insert(symbol, ty);
-        Some(constraints)
+        Some(NarrowingConstraints::from_iter([(symbol, ty)]))
     }
 
     fn evaluate_match_pattern_value(
@@ -521,10 +516,7 @@ impl<'db> NarrowingConstraintsBuilder<'db> {
     ) -> Option<NarrowingConstraints<'db>> {
         let symbol = self.get_subject_symbol(subject)?;
         let ty = infer_same_file_expression_type(self.db, value);
-        let mut constraints = NarrowingConstraints::default();
-        constraints.insert(symbol, ty);
-
-        Some(constraints)
+        Some(NarrowingConstraints::from_iter([(symbol, ty)]))
     }
```

could possibly be done in this PR, or as a followup if you're interested!

---

_@AlexWaygood approved on 2025-03-26 17:33_

Nice, this looks great. A couple of minor comments:

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:86 on 2025-03-28 02:30_

nit: I'd suggest for clarity we use a pure complex (i.e. imaginary) literal here. I spent a while wondering why we get `int  | float | complex` here, and it took me a while to realize that it's because `1+1j` is not really a literal, its a binop Add with int and complex operands.
```suggestion
    case 1j:
        reveal_type(x)  # revealed: complex
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:110 on 2025-03-28 02:30_

```suggestion
    case 1j if reveal_type(x):  # revealed: complex
```

---

_@carljm approved on 2025-03-28 02:32_

Very nice, thank you!

---

_Merged by @carljm on 2025-03-28 02:36_

---

_Closed by @carljm on 2025-03-28 02:36_

---

_Branch deleted on 2025-03-28 04:53_

---
