```yaml
number: 20024
title: "[`flake8-bugbear`] Include certain guaranteed-mutable expressions: tuples, generators, and assignment expressions (`B006`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - rule
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/ruff-20004
created_at: 2025-08-21T16:57:13Z
updated_at: 2025-10-03T14:29:36Z
url: https://github.com/astral-sh/ruff/pull/20024
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-bugbear`] Include certain guaranteed-mutable expressions: tuples, generators, and assignment expressions (`B006`)

---

_Pull request opened by @IDrokin117 on 2025-08-21 16:57_

## Summary
Resolves #20004

The implementation now supports guaranteed-mutable expressions in the following cases:
- Tuple literals with mutable elements (supporting deep nesting)
- Generator expressions
- Named expressions (walrus operator) containing mutable components

Preserves original formatting for assignment value:

```python
# Test case
def f5(x=([1, ])):
    print(x)
```
```python
# Fix before
def f5(x=(None)):
    if x is None:
        x = [1]
    print(x)
```
```python
# Fix after 
def f5(x=None):
    if x is None:
        x = ([1, ])
    print(x)
```
The expansion of detected expressions and the new fixes gated behind previews.

## Test Plan
- Added B006_9.py with a bunch of test cases
- Generated snapshots



---

_Review comment by @IDrokin117 on `crates/ruff_python_codegen/src/generator.rs`:133 on 2025-08-21 17:04_

I've added this method to be able to generate an expression with parentheses. I didn't find a way to ensure it is parenthesized. Using `expr` to generate source code for Tuple, I got
```
tuple_expr = "([1], 2)"
.expr(tuple_expr) # -> "[1], 2", but expected ([1], 2)
```
I am not sure about this, would be appreciated for more sophisticated solution.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:166 on 2025-08-21 17:06_

Would it be reasonable to eliminate all these parameters and pass only the `checker` object? Since this is a private function that will not be utilized elsewhere, using `checker` directly might be more convenient.

---

_@IDrokin117 reviewed on 2025-08-21 17:06_

---

_Comment by @github-actions[bot] on 2025-08-21 17:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-08-29 19:25_

---

_Label `rule` added by @dylwil3 on 2025-09-01 20:54_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:198 on 2025-09-15 20:02_

Is there some reason why the following would be a bad idea? It has the virtue of preserving whatever idiosyncracies were used in the original formatting, as well as type annotations. (I realize this is sort of orthogonal to the PR, but it looks like you had to use `expr_parenthesized` anyway to avoid a syntax error, and this is an alternative approach which feels like an improvement in any case?)

Do you want to try it and see if anything horrible happens in the ecosystem check?

```diff
diff --git i/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs w/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs
index 6a753f94a..ba2b2261b 100644
--- i/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs
+++ w/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs
@@ -5,7 +5,7 @@ use ruff_python_ast::helpers::is_docstring_stmt;
 use ruff_python_ast::name::QualifiedName;
 use ruff_python_ast::parenthesize::parenthesized_range;
 use ruff_python_ast::{self as ast, Expr, ParameterWithDefault};
-use ruff_python_codegen::{Generator, Stylist};
+use ruff_python_codegen::Stylist;
 use ruff_python_index::Indexer;
 use ruff_python_semantic::SemanticModel;
 use ruff_python_semantic::analyze::function_type::is_stub;
@@ -128,7 +128,6 @@ pub(crate) fn mutable_argument_default(checker: &Checker, function_def: &ast::St
                 checker.locator(),
                 checker.stylist(),
                 checker.indexer(),
-                checker.generator(),
                 checker.comment_ranges(),
                 checker.source(),
             ) {
@@ -161,7 +160,6 @@ fn move_initialization(
     locator: &Locator,
     stylist: &Stylist,
     indexer: &Indexer,
-    generator: Generator,
     comment_ranges: &CommentRanges,
     source: &str,
 ) -> Option<Fix> {
@@ -191,12 +189,7 @@ fn move_initialization(
     let _ = write!(&mut content, "if {} is None:", parameter.parameter.name());
     content.push_str(stylist.line_ending().as_str());
     content.push_str(stylist.indentation());
-    let _ = write!(
-        &mut content,
-        "{} = {}",
-        parameter.parameter.name(),
-        generator.expr_parenthesized(default)
-    );
+    let _ = write!(&mut content, "{}", locator.slice(parameter));
     content.push_str(stylist.line_ending().as_str());
 
     // Determine the indentation depth of the function body.

````

---

_@dylwil3 requested changes on 2025-09-15 20:03_

This is great thank you, and sorry for the delay! Can we experiment a bit with the fix?

Also, we should be gating this behind preview - both the expansion of detected expressions and the new fix. If you'd like, you can make these two separate functions in `preview.rs` since we may stabilize them independently.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:166 on 2025-09-15 20:04_

That seems fine to me - go for it!

---

_@dylwil3 reviewed on 2025-09-15 20:04_

---

_@dylwil3 reviewed on 2025-09-15 20:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:198 on 2025-09-15 20:08_

I guess the formatting of assignments is often a little different than for parameter defaults, so you could also keep it closer to the original fix but use the locator to slice out the `default` instead of generating it?

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:198 on 2025-09-23 16:35_

@dylwil3 I've experimented with your proposal and got some results:
1. Using `write!(&mut content, "{}", locator.slice(parameter));` preserves formatting, but, as you mention, variable assignment requires spaces around `=` unlike parameter.
2. Using `locator.slice(default)` will lead to a problem that `generator.expr_parenthesized(default)` tries to solve: `default` doesn't grab parenthesizes. 
```rust
let _ = write!(
    &mut content,
    "{} = {}",
    parameter.parameter.name(),
    locator.slice(default)
);
```
I used `locator.slice` to grab annotation from parameter and place it to variable assignment. What you think?

---

_@IDrokin117 reviewed on 2025-09-23 17:16_

@dylwil3 Thanks for your review! I've pushed some changes according to your points:
1. Added 2 preview flags.
2. Added annotation expr into fix content if any. It is gated behind preview. There already exists a test, so it might solve the annotation issue.

---

_@IDrokin117 reviewed on 2025-09-23 17:17_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:166 on 2025-09-23 17:17_

Done

---

_Review requested from @dylwil3 by @IDrokin117 on 2025-09-23 17:17_

---

_Comment by @IDrokin117 on 2025-09-30 18:56_

@dylwil3 Could you please review the PR again?

---

_Comment by @dylwil3 on 2025-10-01 12:28_

> @dylwil3 Could you please review the PR again?

Yes! Sorry for the delay - I will not be able to get to this till the end of the week, but I have not forgotten!

---

_@dylwil3 reviewed on 2025-10-03 14:25_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:198 on 2025-10-03 14:25_

I pushed a commit to grab the `parenthesized_range` instead which seems to work!

---

_@dylwil3 approved on 2025-10-03 14:25_

Thank you for your patience and for the great contribution!

---

_Label `fixes` added by @dylwil3 on 2025-10-03 14:29_

---

_Label `preview` added by @dylwil3 on 2025-10-03 14:29_

---

_Merged by @dylwil3 on 2025-10-03 14:29_

---

_Closed by @dylwil3 on 2025-10-03 14:29_

---
