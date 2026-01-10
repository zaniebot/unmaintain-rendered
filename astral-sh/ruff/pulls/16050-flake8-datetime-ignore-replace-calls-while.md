```yaml
number: 16050
title: "[`flake8-datetime`] Ignore `.replace()` calls while looking for `.astimezone`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: DTZ005
created_at: 2025-02-09T00:43:58Z
updated_at: 2025-02-09T15:52:31Z
url: https://github.com/astral-sh/ruff/pull/16050
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-datetime`] Ignore `.replace()` calls while looking for `.astimezone`

---

_Pull request opened by @InSyncWithFoo on 2025-02-09 00:43_

## Summary

Resolves #15998.

Previously, this would be considered an error by `DTZ005`, as `parent_expr_is_astimezone()` would only traverse up one level:

```python
datetime.now().replace(...).astimezone()
```

The function will now check for all expressions in the current stack, ignoring all such intermediate `.replace()` calls while looking for `.astimezone`.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-09 00:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2025-02-09 00:59_

The second commit is just a rename. All substantial changes are isolated within [the first](https://github.com/astral-sh/ruff/pull/16050/commits/f51460cf58a2a56b17d1beeb6788a92358f36ade).

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs`:103 on 2025-02-09 12:55_

Could you say a little bit about what this logic branch is for? No tests fail if I apply this diff to your git branch, which makes me suspect that it's not actually reachable (or if it is reachable, that we're missing some test coverage):

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs b/crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs
index 9a430c9e4..53f28cb34 100644
--- a/crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs
+++ b/crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs
@@ -1,4 +1,4 @@
-use ruff_python_ast::{AnyNodeRef, Expr, ExprAttribute, ExprCall};
+use ruff_python_ast::{Expr, ExprAttribute};
 
 use crate::checkers::ast::Checker;
 
@@ -14,7 +14,6 @@ pub(super) enum DatetimeModuleAntipattern {
 /// This assumes that the current expression is a `datetime.datetime` object.
 pub(super) fn followed_by_astimezone(checker: &Checker) -> bool {
     let semantic = checker.semantic();
-    let mut last = None;
 
     for (index, expr) in semantic.current_expressions().enumerate() {
         if index == 0 {
@@ -31,20 +30,15 @@ pub(super) fn followed_by_astimezone(checker: &Checker) -> bool {
             };
 
             match attr.as_str() {
-                "replace" => last = Some(AnyNodeRef::from(expr)),
+                "replace" => continue,
                 "astimezone" => return true,
                 _ => return false,
             }
-        } else {
-            // datetime.now(...).replace(...).astimezone
-            //                          ^^^^^
-            let Expr::Call(ExprCall { func, .. }) = expr else {
-                return false;
-            };
-
-            if !last.is_some_and(|it| it.ptr_eq(AnyNodeRef::from(&**func))) {
-                return false;
-            }
+        }
+        // datetime.now(...).replace(...).astimezone
+        //                          ^^^^^
+        if !expr.is_call_expr() {
+            return false;
         }
     }
```

---

_@AlexWaygood reviewed on 2025-02-09 12:56_

Thank you, this overall looks great. Nicely done!

Thank you for adding comments to the code; it made it easier to understand what was going on. It still took me some time to figure out what was going on here, though. I almost wonder if it's worth adding a comment that links to the Ruff playground showing what the AST we're dealing with here looks like, and explaining that we start at the innermost node in the AST and have to work our way upwards https://play.ruff.rs/89b67ffb-c32a-437e-aa8c-8dcfc8b7068a

---

_@InSyncWithFoo reviewed on 2025-02-09 14:56_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs`:103 on 2025-02-09 14:56_

Fixed a test case:

```python
datetime.now().replace(     # This is not reported
	datetime.now().replace  # But this is
).astimezone()
```

---

_Comment by @AlexWaygood on 2025-02-09 15:45_

OK, I added some documentation for you.

---

_@AlexWaygood approved on 2025-02-09 15:46_

---

_Label `bug` added by @AlexWaygood on 2025-02-09 15:46_

---

_Merged by @AlexWaygood on 2025-02-09 15:48_

---

_Closed by @AlexWaygood on 2025-02-09 15:48_

---

_Branch deleted on 2025-02-09 15:52_

---
