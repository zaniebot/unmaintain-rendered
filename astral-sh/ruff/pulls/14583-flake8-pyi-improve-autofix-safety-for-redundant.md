```yaml
number: 14583
title: "[`flake8-pyi`] Improve autofix safety for `redundant-none-literal` (PYI061)"
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
assignees: []
merged: true
base: main
head: pyi061-fix-syntax-error
created_at: 2024-11-25T14:00:54Z
updated_at: 2024-11-25T17:51:09Z
url: https://github.com/astral-sh/ruff/pull/14583
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Improve autofix safety for `redundant-none-literal` (PYI061)

---

_Pull request opened by @sbrugman on 2024-11-25 14:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Partially resolves #14567
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added regression tests.


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-25 14:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:95 on 2024-11-25 14:05_

`None | None` is invalid _Python_, but it's valid _syntax_ (the code compiles, it just raises an exception!)

```suggestion
        // as `Union[None, None]` is valid Python.
```

---

_@AlexWaygood had review dismissed on 2024-11-25 14:06_

Great, thanks for the quick fix!

---

_@AlexWaygood reviewed on 2024-11-25 14:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:93 on 2024-11-25 14:07_

```suggestion
        // Avoid producing code that would raise an exception
        // when `Literal[None] | None` would be fixed to
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:96 on 2024-11-25 14:07_

```suggestion
        // Avoid producing code that would raise an exception when 
        // `Literal[None] | None` would be fixed to `None | None`.
        // Instead fix to `None`. No action needed for `typing.Union`,
        // as `Union[None, None]` is valid Python.
```

---

_@AlexWaygood reviewed on 2024-11-25 14:07_

---

_Comment by @github-actions[bot] on 2024-11-25 14:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sbrugman on 2024-11-25 14:23_

Note that this fixes the binary cases `Literal[None] | None` and `None | Literal[None]`. Cases where the literal is "sandwiched" between `None`s still produces invalid Python, e.g. `None | Literal[None] | None` and `None | Never | None`. 

The case we just fixed occurs, but rarely: https://github.com/pyapp-kit/in-n-out/blob/main/src/in_n_out/_global.py#L246. The sandwiching I could not find a single example of.

---

_Comment by @sbrugman on 2024-11-25 14:33_

I'll fix the sandwiching case for RUF020 and PYI061 in a follow-up PR.

---

_Label `fixes` added by @MichaReiser on 2024-11-25 14:42_

---

_Comment by @AlexWaygood on 2024-11-25 14:48_

I just took a look at it, and the sandwiching case seems like it might be harder to fix, at least without copying over some of the logic from PYI016 into this rule (or moving some of the PYI016 logic to a common module that both rules import). I'd be okay if we said we should simply not offer a fix in that situation. We could also put a note in the docs for this rule that we recommend also enabling PYI016 if you enable this rule.

What do you think?

---

_Comment by @AlexWaygood on 2024-11-25 15:14_

It might also be worth adding this as a test snippet with a big comment next to it:

```py
a: int | Literal[None] | None
```

This snippet still gets autofixed to

```py
a: int | None | None
```

But that's actually okay! That union desugars to `(int | None) | None`, and the left-hand-side operand there is a `types.UnionType` instance, which has a `__or__` implementation that accepts `None`.

---

_Comment by @sbrugman on 2024-11-25 15:15_

The catch is that not offering a fix in cases where we detect a PEP604-style `None` union sandwich is almost equally difficult as fixing it. 

Guaranteeing the fix is safe requires checking the entire top-level union. Simply checking the parent or grandparent expression is not enough (although these will never occur in reality):
```python
d: None | ((None | Literal[None]) | None) | None
```

I'd opt to acknowledge this as a "known limitation" in the docs and to add these test cases for posterity (red-knot).

Is there any precedent of marking a rule as Unsafe when another rule is disabled? 

---

_Comment by @AlexWaygood on 2024-11-25 15:18_

> although these will never occur in reality

I'd be a bit more cautious there. I feel like this kind of thing isn't _that_ strange, and could easily appear in generated stubs from a tool like https://github.com/nipunn1313/mypy-protobuf, for example.

---

_Comment by @AlexWaygood on 2024-11-25 15:33_

I think it wouldn't be _too_ complex to detect if there are any bare `None`s in the union, and then avoid adding an autofix if so. Something like this?

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
index 1f8bfb480..b060d3cd4 100644
--- a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
+++ b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
@@ -1,7 +1,10 @@
 use ruff_diagnostics::{Applicability, Diagnostic, Edit, Fix, FixAvailability, Violation};
 use ruff_macros::{derive_message_formats, violation};
 use ruff_python_ast::{Expr, ExprBinOp, ExprNoneLiteral, Operator};
-use ruff_python_semantic::analyze::typing::traverse_literal;
+use ruff_python_semantic::{
+    analyze::typing::{traverse_literal, traverse_union},
+    SemanticModel,
+};
 use ruff_text_size::Ranged;
 
 use smallvec::SmallVec;
@@ -90,35 +93,14 @@ pub(crate) fn redundant_none_literal<'a>(checker: &mut Checker, literal_expr: &'
     let fix = if other_literal_elements_seen {
         None
     } else {
-        // Avoid producing code that would raise an exception when
-        // `Literal[None] | None` would be fixed to `None | None`.
-        // Instead fix to `None`. No action needed for `typing.Union`,
-        // as `Union[None, None]` is valid Python.
-        // See https://github.com/astral-sh/ruff/issues/14567.
-        let replacement_range = if let Some(Expr::BinOp(ExprBinOp {
-            left,
-            op: Operator::BitOr,
-            right,
-            range: parent_range,
-        })) = checker.semantic().current_expression_parent()
-        {
-            if matches!(**left, Expr::NoneLiteral(_)) || matches!(**right, Expr::NoneLiteral(_)) {
-                *parent_range
-            } else {
-                literal_expr.range()
-            }
-        } else {
-            literal_expr.range()
-        };
-
-        Some(Fix::applicable_edit(
-            Edit::range_replacement("None".to_string(), replacement_range),
-            if checker.comment_ranges().intersects(literal_expr.range()) {
+        create_fix_edit(checker.semantic(), literal_expr).map(|edit| {
+            let applicability = if checker.comment_ranges().intersects(literal_expr.range()) {
                 Applicability::Unsafe
             } else {
                 Applicability::Safe
-            },
-        ))
+            };
+            Fix::applicable_edit(edit, applicability)
+        })
     };
 
     for none_expr in none_exprs {
@@ -134,3 +116,44 @@ pub(crate) fn redundant_none_literal<'a>(checker: &mut Checker, literal_expr: &'
         checker.diagnostics.push(diagnostic);
     }
 }
+
+fn create_fix_edit(semantic: &SemanticModel, literal_expr: &Expr) -> Option<Edit> {
+    // Avoid producing code that would raise an exception when
+    // `Literal[None] | None` would be fixed to `None | None`.
+    // Instead fix to `None`. No action needed for `typing.Union`,
+    // as `Union[None, None]` is valid Python.
+    // See https://github.com/astral-sh/ruff/issues/14567.
+    let mut enclosing_union = None;
+    let mut expression_ancestors = semantic.current_expressions().skip(1);
+    let mut parent_expr = expression_ancestors.next();
+    while let Some(Expr::BinOp(ExprBinOp {
+        op: Operator::BitOr,
+        ..
+    })) = parent_expr
+    {
+        enclosing_union = parent_expr;
+        parent_expr = expression_ancestors.next();
+    }
+
+    let mut is_union_with_bare_none = false;
+    if let Some(enclosing_union) = enclosing_union {
+        traverse_union(
+            &mut |expr, _| {
+                if matches!(expr, Expr::NoneLiteral(_)) {
+                    is_union_with_bare_none = true;
+                }
+            },
+            semantic,
+            enclosing_union,
+        );
+    }
+
+    if is_union_with_bare_none {
+        None
+    } else {
+        Some(Edit::range_replacement(
+            "None".to_string(),
+            literal_expr.range(),
+        ))
+    }
+}
```

---

_Comment by @AlexWaygood on 2024-11-25 15:44_

(Sorry, the patch I posted above had a bug in it initially. I have now tested it, and fixed the bug...)

Also, it still doesn't handle cases like `Literal[None] | Literal[None]`. Though I think that could be fixed by fiddling with the closure I pass into `traverse_union()` in that patch.

---

_Comment by @sbrugman on 2024-11-25 15:47_

That solution works, I'll have a look. I was considering doing it similarly, but inside the union visitor (which checks if there is a nested union), but it's pretty similar functionally.

In the end it's not worth spending too much on this as I assume that when red-knot is in place this all will be a single rule that detects annotations that can be simplified.

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-25 17:35_

---

_@AlexWaygood approved on 2024-11-25 17:36_

Thanks!!

---

_Merged by @AlexWaygood on 2024-11-25 17:40_

---

_Closed by @AlexWaygood on 2024-11-25 17:40_

---

_Branch deleted on 2024-11-25 17:51_

---
