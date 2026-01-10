```yaml
number: 16027
title: "[`pyupgrade`] Comments within parenthesized value ranges should not affect applicability (`UP040`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
assignees: []
merged: true
base: main
head: UP040
created_at: 2025-02-07T17:16:29Z
updated_at: 2025-02-07T20:44:47Z
url: https://github.com/astral-sh/ruff/pull/16027
synced_at: 2026-01-10T19:57:22Z
```

# [`pyupgrade`] Comments within parenthesized value ranges should not affect applicability (`UP040`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-07 17:16_

## Summary

Follow-up to #16026.

Previously, the fix for this would be marked as unsafe, even though all comments are preserved:

```python
# .pyi
T: TypeAlias = (  # Comment
	int | str
)
```

Now it is safe: comments within the parenthesized range no longer affect applicability.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-07 17:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2025-02-07 18:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:287 on 2025-02-07 18:34_

thank you! I actually feel like I'd find it easier to understand this if we just put all the applicability logic in this function. Something like this (relative to your branch)?

```diff
diff --git a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs
index da34672c9..777bb3184 100644
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs
@@ -1,5 +1,4 @@
 use itertools::Itertools;
-use std::cmp;
 
 use ruff_diagnostics::{Applicability, Diagnostic, Edit, Fix, FixAvailability, Violation};
 use ruff_macros::{derive_message_formats, ViolationMetadata};
@@ -177,7 +176,6 @@ pub(crate) fn non_pep695_type_alias_type(checker: &Checker, stmt: &StmtAssign) {
         &target_name.id,
         value,
         &vars,
-        Applicability::Safe,
         TypeAliasKind::TypeAliasType,
     ));
 }
@@ -237,13 +235,6 @@ pub(crate) fn non_pep695_type_alias(checker: &Checker, stmt: &StmtAnnAssign) {
         name,
         value,
         &vars,
-        // The fix is only safe in a type stub because new-style aliases have different runtime behavior
-        // See https://github.com/astral-sh/ruff/issues/6434
-        if checker.source_type.is_stub() {
-            Applicability::Safe
-        } else {
-            Applicability::Unsafe
-        },
         TypeAliasKind::TypeAlias,
     ));
 }
@@ -255,7 +246,6 @@ fn create_diagnostic(
     name: &Name,
     value: &Expr,
     type_vars: &[TypeVar],
-    base_applicability: Applicability,
     type_alias_kind: TypeAliasKind,
 ) -> Diagnostic {
     let source = checker.source();
@@ -272,20 +262,28 @@ fn create_diagnostic(
     );
     let edit = Edit::range_replacement(content, stmt.range());
 
-    // it would be easier to check for comments in the whole `stmt.range`, but because
-    // `create_diagnostic` uses the full source text of `value`, comments within `value` are
-    // actually preserved. thus, we have to check for comments in `stmt` but outside of `value`
-    let pre_value = TextRange::new(stmt.start(), range_with_parentheses.start());
-    let post_value = TextRange::new(range_with_parentheses.end(), stmt.end());
     let applicability =
-        if comment_ranges.intersects(pre_value) || comment_ranges.intersects(post_value) {
+        if type_alias_kind == TypeAliasKind::TypeAlias && !checker.source_type.is_stub() {
+            // The fix is always unsafe in non-stubs stub
+            // because new-style aliases have different runtime behavior at runtime.
+            // See https://github.com/astral-sh/ruff/issues/6434
             Applicability::Unsafe
         } else {
-            Applicability::Safe
+            // In stub files, or in non-stub files for `TypeAliasType` assignments,
+            // the fix is only unsafe if it would delete comments.
+            //
+            // it would be easier to check for comments in the whole `stmt.range`, but because
+            // `create_diagnostic` uses the full source text of `value`, comments within `value` are
+            // actually preserved. thus, we have to check for comments in `stmt` but outside of `value`
+            let pre_value = TextRange::new(stmt.start(), range_with_parentheses.start());
+            let post_value = TextRange::new(range_with_parentheses.end(), stmt.end());
+            if comment_ranges.intersects(pre_value) || comment_ranges.intersects(post_value) {
+                Applicability::Unsafe
+            } else {
+                Applicability::Safe
+            }
         };
 
-    let fix = Fix::applicable_edit(edit, cmp::min(applicability, base_applicability));
-
     Diagnostic::new(
         NonPEP695TypeAlias {
             name: name.to_string(),
@@ -293,5 +291,5 @@ fn create_diagnostic(
         },
         stmt.range(),
     )
-    .with_fix(fix)
+    .with_fix(Fix::applicable_edit(edit, applicability))
 }
```

---

_Label `fixes` added by @dylwil3 on 2025-02-07 19:13_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:268 on 2025-02-07 20:33_

```suggestion
            // The fix is always unsafe in non-stubs
            // because new-style aliases have different runtime behavior.
```

---

_@dylwil3 requested changes on 2025-02-07 20:36_

Couple typos but otherwise lgtm!

---

_@dylwil3 approved on 2025-02-07 20:44_

---

_Merged by @dylwil3 on 2025-02-07 20:44_

---

_Closed by @dylwil3 on 2025-02-07 20:44_

---

_Branch deleted on 2025-02-07 20:44_

---
