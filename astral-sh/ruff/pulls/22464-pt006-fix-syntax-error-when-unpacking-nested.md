```yaml
number: 22464
title: "[PT006] Fix syntax error when unpacking nested tuples in parametrize fixes (#22441)"
type: pull_request
state: open
author: bxff
labels:
  - rule
  - fixes
assignees: []
base: main
head: fix-pt006-clean
created_at: 2026-01-08T19:56:23Z
updated_at: 2026-01-09T15:21:12Z
url: https://github.com/astral-sh/ruff/pull/22464
synced_at: 2026-01-12T15:57:50Z
```

# [PT006] Fix syntax error when unpacking nested tuples in parametrize fixes (#22441)

---

_@bxff_

## Summary

Fixes a syntax error bug in the `PT006` rule where applying fixes could generate invalid Python code.

**Problem**: When unpacking nested tuples in `pytest.mark.parametrize` decorators, Ruff's code generator unparses tuples without outer parentheses at level 0 (e.g., `(1, 2)` becomes `1, 2` and `((1,),)` becomes `(1,),`). This caused syntax errors like `[1, 2,]` or `[(1,),,]` when used as list elements.

**Solution**: Introduced two helper functions in `parametrize.rs`:
- `is_parenthesized`: Validates if a string is fully enclosed in matching parentheses
- `unparse_expr_in_sequence`: Ensures non-empty tuples are always parenthesized when used as sequence elements

Fixes https://github.com/astral-sh/ruff/issues/22441

## Test Plan

- Added comprehensive regression tests to `PT006.py` covering:
  - Single-element nested empty tuples: `((),)`
  - Single-element nested single-value tuples: `((1,),)`
  - Single-element nested multi-value tuples: `((1, 2),)`
  - Deeply nested structures: `(((1,),),)`
  - Mixed types: `("hello",,)`, `([1, 2],,)`

- Verified edge cases with automated snapshot testing:
  - All 47 existing flake8-pytest-style tests continue to pass
  - Updated snapshots: `PT006_default.snap`, `PT006_csv.snap`, `PT006_list.snap`

---

_Label `rule` added by @amyreese on 2026-01-08 20:06_

---

_Label `fixes` added by @amyreese on 2026-01-08 20:06_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 20:17_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@chirizxc reviewed on 2026-01-09 15:01_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:790 on 2026-01-09 15:01_

maybe:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/par
ametrize.rs
index d1a5240a15..537687d571 100644
--- a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs
+++ b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs
@@ -753,41 +753,19 @@ fn unpack_single_element_items(checker: &Checker, expr: &Expr) -> Vec<Edit> {
 
 fn unparse_expr_in_sequence(expr: &Expr, checker: &Checker) -> String {
     let content = checker.generator().expr(expr);
-    // The generator may unparse tuples without outer parentheses at level 0.
-    // For example, `(1, 2)` becomes `1, 2` and `((1,),)` becomes `(1,),`.
-    // When these are used as elements in a list, it causes syntax errors.
-    // We detect this by checking if the content ends with a trailing comma
-    // that is not inside parentheses.
+
     if let Expr::Tuple(tuple) = expr {
-        if !tuple.is_empty() && !is_parenthesized(&content) {
+        if !tuple.is_empty() && parenthesized_range(
+            expr.into(),
+            checker.semantic().current_statement().into(),
+            checker.tokens()
+        ).is_none() {
             return format!("({content})");
         }
     }
     content
 }
 
-/// Check if the content is fully parenthesized (starts with '(' and the matching ')' is at the end).
-fn is_parenthesized(content: &str) -> bool {
-    if !content.starts_with('(') {
-        return false;
-    }
-    let mut depth = 0;
-    for (i, c) in content.chars().enumerate() {
-        match c {
-            '(' => depth += 1,
-            ')' => {
-                depth -= 1;
-                if depth == 0 {
-                    // If we close the first paren before the end, it's not fully parenthesized
-                    return i == content.len() - 1;
-                }
-            }
-            _ => {}
-        }
-    }
-    false
-}
-
 fn handle_value_rows(
     checker: &Checker,
     elts: &[Expr],
```

---

_Review comment by @bxff on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:790 on 2026-01-09 15:21_

`parenthesized_range` has a subtle edge case: with extra grouping parens like `(((1,)),)`, the AST is unchanged so it would skip adding outer parens, but the generator still outputs `1,` which needs them.

---

_@bxff reviewed on 2026-01-09 15:21_

---
