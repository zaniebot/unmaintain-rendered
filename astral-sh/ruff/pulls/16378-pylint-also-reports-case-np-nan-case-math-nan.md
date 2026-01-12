```yaml
number: 16378
title: "[`pylint`] Also reports `case np.nan`/`case math.nan` (`PLW0177`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: PLW0177
created_at: 2025-02-25T17:54:22Z
updated_at: 2025-02-26T18:50:58Z
url: https://github.com/astral-sh/ruff/pull/16378
synced_at: 2026-01-12T15:55:54Z
```

# [`pylint`] Also reports `case np.nan`/`case math.nan` (`PLW0177`)

---

_@InSyncWithFoo_

## Summary

Resolves #16374.

`PLW0177` now also reports the pattern of a case branch if it is an attribute access whose qualified name is that of either `np.nan` or `math.nan`.

As the rule is in preview, the changes are not preview-gated.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-25 18:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-02-25 20:14_

---

_Comment by @MichaReiser on 2025-02-26 09:04_

@ntBre could you take a look at this PR?

---

_@InSyncWithFoo reviewed on 2025-02-26 15:44_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pylint/nan_comparison.py`:64 on 2025-02-26 15:44_

No reason, really; I just wanted to make sure I didn't mess up the logic.

---

_@InSyncWithFoo reviewed on 2025-02-26 15:46_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pylint/nan_comparison.py`:66 on 2025-02-26 15:46_

The existing logic checks for such cases in comparisons (e.g., `a == b != c`), but not `match` patterns. `case float('nan')` does not invoke `float`, while `case npy_nan` binds the matched expression to that name.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/nan_comparison.py`:64 on 2025-02-26 15:54_

These caused a `TypeError` when I tried them. Is there a reason to test these specifically?

---

_@ntBre reviewed on 2025-02-26 15:54_

---

_@ntBre reviewed on 2025-02-26 15:56_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/nan_comparison.py`:66 on 2025-02-26 15:56_

These were a bit confusing for me because I forgot how `match` works again. I'm not sure if it's worth a comment saying that the first one doesn't match because it's a `MatchClass` pattern and the second is just a variable or if most readers will remember that. I'll leave it up to you.

---

_@ntBre reviewed on 2025-02-26 15:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1748 on 2025-02-26 15:57_

nit: I usually ignore all fields with `..`
```suggestion
           cases,
            ..
```

With any luck that will allow it to fit on one line too.

---

_@ntBre reviewed on 2025-02-26 16:05_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/nan_comparison.py`:66 on 2025-02-26 16:05_

Oops, I thought I hit "start review" before I deleted the comment. I realized that I was just confused about `match` again. Thanks for the clarification!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nan_comparison.rs`:51 on 2025-02-26 16:07_

I think what you have here is fine, but another possible refactor is to pull out `nan_comparison_impl` for example, with the signature

```rust
fn nan_comparison_impl<'a>(checker: &Checker, comparators: impl Iterator<Item = &'a Expr>) {
```

and call it with this chained iterator in `nan_comparison` and make `nan_comparison_match` look like this. 

```rust
pub(crate) fn nan_comparison_match(checker: &Checker, cases: &[ast::MatchCase]) {
    nan_comparison_impl(
        checker,
        cases.iter().filter_map(|case| {
            case.pattern
                .as_match_value()
                .and_then(|pattern| Some(&*pattern.value))
        }),
    );
}
```

Here's a patch relative to your branch, but the same approach relative to the original code would probably give a smaller diff.

<details>
<summary>Patch</summary><p>

```diff
diff --git a/crates/ruff_linter/src/rules/pylint/rules/nan_comparison.rs b/crates/ruff_linter/src/rules/pylint/rules/nan_comparison.rs
index 991a84bb4..90e443c5d 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/nan_comparison.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/nan_comparison.rs
@@ -48,7 +48,23 @@ impl Violation for NanComparison {
 
 /// PLW0177
 pub(crate) fn nan_comparison(checker: &Checker, left: &Expr, comparators: &[Expr]) {
-    for expr in std::iter::once(left).chain(comparators) {
+    nan_comparison_impl(checker, std::iter::once(left).chain(comparators));
+}
+
+/// PLW0177
+pub(crate) fn nan_comparison_match(checker: &Checker, cases: &[ast::MatchCase]) {
+    nan_comparison_impl(
+        checker,
+        cases.iter().filter_map(|case| {
+            case.pattern
+                .as_match_value()
+                .and_then(|pattern| Some(&*pattern.value))
+        }),
+    );
+}
+
+fn nan_comparison_impl<'a>(checker: &Checker, comparators: impl Iterator<Item = &'a Expr>) {
+    for expr in comparators {
         if expr.is_call_expr() {
             if is_nan_float(expr, checker.semantic()) {
                 checker.report_diagnostic(Diagnostic::new(
@@ -65,19 +81,6 @@ pub(crate) fn nan_comparison(checker: &Checker, left: &Expr, comparators: &[Expr
     }
 }
 
-/// PLW0177
-pub(crate) fn nan_comparison_match(checker: &Checker, cases: &[ast::MatchCase]) {
-    for case in cases {
-        let ast::Pattern::MatchValue(pattern) = &case.pattern else {
-            continue;
-        };
-
-        if let Some(nan) = Nan::from(&pattern.value, checker.semantic()) {
-            checker.report_diagnostic(Diagnostic::new(NanComparison { nan }, pattern.range));
-        }
-    }
-}
-
 #[derive(Debug, PartialEq, Eq)]
 enum Nan {
     /// `math.isnan`
```
</p></details>

---

_@ntBre approved on 2025-02-26 16:09_

Thanks for your work on this and for the clarification on my `match` comments! I left a slightly different implementation strategy suggestion for your consideration, but I'm happy with this one too if you are.

---

_@InSyncWithFoo reviewed on 2025-02-26 16:18_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1748 on 2025-02-26 16:18_

I would write that too, but other reviewers (notably Alex) prefer `field: _` over `..` since the former would be caught by the compiler if something were to be changed.

---

_@ntBre reviewed on 2025-02-26 16:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1748 on 2025-02-26 16:20_

Ah okay, fair enough!

---

_@ntBre approved on 2025-02-26 18:50_

Nice, thanks!

---

_Merged by @ntBre on 2025-02-26 18:50_

---

_Closed by @ntBre on 2025-02-26 18:50_

---

_Branch deleted on 2025-02-26 18:50_

---
