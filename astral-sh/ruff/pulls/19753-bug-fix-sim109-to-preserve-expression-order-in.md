```yaml
number: 19753
title: ":bug: Fix SIM109 to preserve expression order in boolean operations"
type: pull_request
state: closed
author: mikeleppane
labels: []
assignees: []
base: main
head: fix(#18945)/SIM109-fix-reorders-statements-incorrectly
created_at: 2025-08-05T07:14:14Z
updated_at: 2025-11-21T08:07:18Z
url: https://github.com/astral-sh/ruff/pull/19753
synced_at: 2026-01-12T15:56:46Z
```

# :bug: Fix SIM109 to preserve expression order in boolean operations

---

_@mikeleppane_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

### Problem
* The `compare_with_tuple` rule [(SIM109)](https://docs.astral.sh/ruff/rules/compare-with-tuple/#compare-with-tuple-sim109) was incorrectly reordering expressions when transforming multiple equality comparisons, which could change the semantic meaning due to Python's short-circuit evaluation. For example:
```python
# Before (incorrect transformation)
x or y == z or y == w  →  y in (z, w) or x  # Wrong order!

# After (correct transformation) 
x or y == z or y == w  →  x or y in (z, w)  # Preserves order
```

### Solution
* Fixed expression ordering: The replacement logic now preserves the original order by inserting the optimized expression at the position of the first matching equality comparison


### Changes
* Modified the replacement expression building logic in `compare_with_tuple()`
* Added order-preserving iteration that maintains the original boolean expression structure


Related issue: [#18945](https://github.com/astral-sh/ruff/issues/18945)

## Test Plan

Enhanced snapshot tests by adding three more cases.


---

_Comment by @github-actions[bot] on 2025-08-05 07:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:08_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM109.py`:26 on 2025-08-11 20:15_

I think this OK might be wrong since this case gets a diagnostic in the snapshot (or the snapshot result/implementation is wrong).

---

_@ntBre reviewed on 2025-08-11 20:59_

Thanks for working on this!

I'm not sure I'm quite following the new logic. Would something like this handle all of the new tests?

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs b/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs
index 7576c71fc1..d18069b724 100644
--- a/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs
+++ b/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs
@@ -547,6 +547,14 @@ pub(crate) fn compare_with_tuple(checker: &Checker, expr: &Expr) {
             continue;
         }
 
+        let Some(node_range) = comparators
+            .iter()
+            .map(|expr| expr.range())
+            .min_by_key(|range| range.start())
+        else {
+            continue;
+        };
+
         // Create a `x in (a, b)` expression.
         let node = ast::ExprTuple {
             elts: comparators.into_iter().cloned().collect(),
@@ -565,7 +573,7 @@ pub(crate) fn compare_with_tuple(checker: &Checker, expr: &Expr) {
             left: Box::new(node1.into()),
             ops: Box::from([CmpOp::In]),
             comparators: Box::from([node.into()]),
-            range: TextRange::default(),
+            range: node_range,
             node_index: ruff_python_ast::AtomicNodeIndex::dummy(),
         };
         let in_expr = node2.into();
@@ -575,7 +583,7 @@ pub(crate) fn compare_with_tuple(checker: &Checker, expr: &Expr) {
             },
             expr.range(),
         );
-        let unmatched: Vec<Expr> = values
+        let mut unmatched: Vec<Expr> = values
             .iter()
             .enumerate()
             .filter(|(index, _)| !indices.contains(index))
@@ -584,10 +592,13 @@ pub(crate) fn compare_with_tuple(checker: &Checker, expr: &Expr) {
         let in_expr = if unmatched.is_empty() {
             in_expr
         } else {
+            unmatched.push(in_expr);
+            unmatched.sort_by_key(|expr| expr.start());
+
             // Wrap in a `x in (a, b) or ...` boolean operation.
             let node = ast::ExprBoolOp {
                 op: BoolOp::Or,
-                values: iter::once(in_expr).chain(unmatched).collect(),
+                values: unmatched,
                 range: TextRange::default(),
                 node_index: ruff_python_ast::AtomicNodeIndex::dummy(),
             };
```

The idea is just sorting the `in_expr` and the `unmatched` expressions by their starting ranges instead of always chaining them in the same order. It seemed a little easier to understand, at least to me, if it handles every case we need to handle.

We may also want to document the limitation, which I think applies to both approaches. Namely, there's not a great solution if the `unmatched` comparison falls in between two `matches`. In my patch, this is the decision between `min` and `max` basically. For example, should:

```py
y == z or x or y == w
```

become:

```py
x or y in (z, w)
```

or

```py
y in (z, w) or x
```

I believe we current prefer the latter, which is fine, and part of the reason the fix is unsafe, but we could document that as part of the `## Fix safety` section or even `## Known issues` section, which we have in some rules. `Issues` feels a little strong for this case, though.

---

_Comment by @MichaReiser on 2025-11-21 08:07_

Thanks for your submission.

I'll close this PR due to inactivity but we'd be more than happy to review a resubmission that answers Brent's question.

---

_Closed by @MichaReiser on 2025-11-21 08:07_

---
