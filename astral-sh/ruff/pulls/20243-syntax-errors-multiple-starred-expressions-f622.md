```yaml
number: 20243
title: "[syntax-errors]: multiple-starred-expressions (F622)"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
assignees: []
merged: true
base: main
head: F622
created_at: 2025-09-04T16:36:26Z
updated_at: 2025-09-24T19:41:31Z
url: https://github.com/astral-sh/ruff/pull/20243
synced_at: 2026-01-10T17:40:28Z
```

# [syntax-errors]: multiple-starred-expressions (F622)

---

_Pull request opened by @11happy on 2025-09-04 16:36_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements https://docs.astral.sh/ruff/rules/multiple-starred-expressions/ as a semantic syntax error

## Test Plan

 I have added inline tests as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-09-04 16:36_

---

_Review requested from @dhruvmanila by @11happy on 2025-09-04 16:36_

---

_Renamed from "feat: implement F622" to "[syntax-errors]: multiple-starred-expressions (F622)" by @11happy on 2025-09-04 16:39_

---

_Comment by @11happy on 2025-09-07 16:14_

Hii @ntBre , would love your feedback on this when you get a chance. 
Thank you : )

---

_Comment by @github-actions[bot] on 2025-09-07 16:21_

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

_Comment by @ntBre on 2025-09-07 18:22_

Thanks for the pings. Your PRs are in my inbox, but we've been focused on the minor release. I'll get to them soon.

---

_Comment by @11happy on 2025-09-07 18:48_

Sure, No worries at all, I completely understand.  Thank you for your time : )

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1458 on 2025-09-12 17:05_

Could we add some documentation for this? I think this one could justify a larger writeup (like in the F622 docs) than some of the simpler errors like `YieldFromInAsyncFunction` directly above.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:107 on 2025-09-12 17:07_

I don't think this check is quite right. There's a new ecosystem hit for a case like this:

```py
(*_, normed), *_ = [(1,), 2]
```

which should be allowed. You may be able to take inspiration from the actual F622 implementation (which we'll also need to delete before adding this second implementation, or we'll get duplicate diagnostics. That's the reason for the change in the F622.py snapshot above).

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1272 on 2025-09-12 17:08_

I'd prefer not to reformat any unrelated code, but I guess if the formatting CI job doesn't complain it's okay.

---

_@ntBre requested changes on 2025-09-12 17:09_

Thanks for your work here! I think we need to fix a false positive in the ecosystem check, avoid duplicate diagnostics, and add a bit of documentation.

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1458 on 2025-09-14 15:46_

done

---

_@11happy reviewed on 2025-09-14 15:46_

---

_@11happy reviewed on 2025-09-14 15:46_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:107 on 2025-09-14 15:46_

done

---

_@11happy reviewed on 2025-09-14 15:47_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1272 on 2025-09-14 15:47_

Sure

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:216 on 2025-09-15 21:25_

Is this argument still used for anything, or could we delete the argument and the code that checks it too?

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:373 on 2025-09-15 21:51_

I think instead of recursing here, we could perform this `multiple_star_expression` check in `check_expr` instead of `check_stmt`. The parent visitor should already recurse automatically into the child expressions. That seems closer to how F622 was implemented originally. Concretely, I'd suggest something like this, which also uses the structure of the original F622 implementation:

<details><summary>Patch</summary>

```diff
diff --git a/crates/ruff_python_parser/src/semantic_errors.rs b/crates/ruff_python_parser/src/semantic_errors.rs
index 1a8fb3ad6f..b4c0ffb947 100644
--- a/crates/ruff_python_parser/src/semantic_errors.rs
+++ b/crates/ruff_python_parser/src/semantic_errors.rs
@@ -104,9 +104,6 @@ impl SemanticSyntaxChecker {
                         *range,
                     );
                 }
-                for target in targets {
-                    Self::multiple_star_expression(target, ctx);
-                }
 
                 // test_ok assign_stmt_starred_expr_value
                 // _ = 4
@@ -361,38 +358,6 @@ impl SemanticSyntaxChecker {
             );
         }
     }
-    fn multiple_star_expression<Ctx: SemanticSyntaxContext>(expr: &Expr, ctx: &Ctx) {
-        match expr {
-            Expr::Tuple(ast::ExprTuple { elts, range, .. })
-            | Expr::List(ast::ExprList { elts, range, .. }) => {
-                let mut count = 0;
-                for elt in elts {
-                    if matches!(elt, Expr::Starred(_)) {
-                        count += 1;
-                    }
-                    Self::multiple_star_expression(elt, ctx);
-                }
-                if count > 1 {
-                    // test_err multiple_starred_assignment_target
-                    // (*a, *b) = (1, 2)
-                    // [*a, *b] = (1, 2)
-                    // (*a, *b, c) = (1, 2, 3)
-                    // [*a, *b, c] = (1, 2, 3)
-
-                    // test_ok multiple_starred_assignment_target
-                    // (*a, b) = (1, 2)
-                    // (*_, normed), *_ = [(1,), 2]
-
-                    Self::add_error(
-                        ctx,
-                        SemanticSyntaxErrorKind::MultipleStarredExpressions,
-                        *range,
-                    );
-                }
-            }
-            _ => {}
-        }
-    }
 
     /// Check for [`SemanticSyntaxErrorKind::WriteToDebug`] in `stmt`.
     fn debug_shadowing<Ctx: SemanticSyntaxContext>(stmt: &ast::Stmt, ctx: &Ctx) {
@@ -765,6 +730,45 @@ impl SemanticSyntaxChecker {
             }) => {
                 Self::duplicate_parameter_name(parameters, ctx);
             }
+            Expr::Tuple(ast::ExprTuple {
+                elts,
+                ctx: expr_ctx,
+                range,
+                node_index: _,
+                parenthesized: _,
+            })
+            | Expr::List(ast::ExprList {
+                elts,
+                ctx: expr_ctx,
+                range,
+                node_index: _,
+            }) => {
+                if expr_ctx.is_store() {
+                    let mut has_starred: bool = false;
+                    for elt in elts {
+                        if elt.is_starred_expr() {
+                            if has_starred {
+                                // test_err multiple_starred_assignment_target
+                                // (*a, *b) = (1, 2)
+                                // [*a, *b] = (1, 2)
+                                // (*a, *b, c) = (1, 2, 3)
+                                // [*a, *b, c] = (1, 2, 3)
+
+                                // test_ok multiple_starred_assignment_target
+                                // (*a, b) = (1, 2)
+                                // (*_, normed), *_ = [(1,), 2]
+                                Self::add_error(
+                                    ctx,
+                                    SemanticSyntaxErrorKind::MultipleStarredExpressions,
+                                    *range,
+                                );
+                                return;
+                            }
+                            has_starred = true;
+                        }
+                    }
+                }
+            }
             _ => {}
         }
     }
```

</details>

That's pretty deeply indented, so it might still be a good idea to factor out a function.

Along these lines, another useful test case might be something like this:

```py
(*a, *b, (*c, *d)) = (1, 2)
```

We currently emit 2 F622 diagnostics for this, including with your changes. That would help make sure we get the recursion in somewhere. At first I thought we could just delete the recursive `multiple_star_expression` call.

---

_@ntBre reviewed on 2025-09-15 21:58_

Thanks! I think the tests look good now, just a couple of simplification suggestions.

---

_@11happy reviewed on 2025-09-22 12:51_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:216 on 2025-09-22 12:51_

done, yes I checked it can be removed.

---

_@11happy reviewed on 2025-09-22 12:51_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:373 on 2025-09-22 12:51_

Thank you , I have updated it accordingly : )

---

_Comment by @11happy on 2025-09-22 16:32_

The `cargo test (wasm)` failure seems unrelated to this PR changes.

Thank you

---

_Comment by @11happy on 2025-09-24 16:56_

Please let me know if there's any more changes required for this PR.

Thank you : )

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:392 on 2025-09-24 19:23_

tiny nit:


```suggestion

    fn multiple_star_expression<Ctx: SemanticSyntaxContext>(
```

---

_@ntBre approved on 2025-09-24 19:28_

Thank you! I just had one tiny spacing nit that I'll apply and merge.

---

_Label `internal` added by @ntBre on 2025-09-24 19:28_

---

_Merged by @ntBre on 2025-09-24 19:32_

---

_Closed by @ntBre on 2025-09-24 19:32_

---
