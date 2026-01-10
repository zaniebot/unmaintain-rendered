```yaml
number: 15254
title: "[flake8-bugbear] Catch yield in subexpressions (B901) (#14453)"
type: pull_request
state: closed
author: kaspell
labels:
  - rule
  - preview
assignees: []
base: main
head: refactor/b901_missing_yield_subexpressions
created_at: 2025-01-04T12:40:32Z
updated_at: 2025-01-21T18:02:35Z
url: https://github.com/astral-sh/ruff/pull/15254
synced_at: 2026-01-10T20:05:43Z
```

# [flake8-bugbear] Catch yield in subexpressions (B901) (#14453)

---

_Pull request opened by @kaspell on 2025-01-04 12:40_

## Summary

Currently, the B901 rule misses yield expressions that are not top-of-tree in a function body. Refactor the rule to find such yield expressions, but otherwise try to follow the flake8-bugbear implementation. Do not search for yield or yield from expressions in assignment statements, or in lambda or function call expressions.

Closes #14453.

## Test Plan

`cargo test`. Didn't yet manage to get the ecosystem tests to run locally.

---

_Comment by @github-actions[bot] on 2025-01-04 12:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-01-06 09:22_

---

_Label `preview` added by @MichaReiser on 2025-01-06 09:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-06 10:17_

Thanks. I think we can simplify the implementation a fair bit by implementing `Visitor` instead of `StatementVisitor`. The difference is that `Visitor` visits expressionos by default so that you don't have to write that code yourself. This also guarantees that we find `yield` expressions in arbitary nested expressions. 

I also took a quick look at the upstream [bugbear implementation](https://github.com/PyCQA/flake8-bugbear/blob/3a140377c8f1f585013a1566f2c8bb3ead9c329c/bugbear.py#L1256-L1291) and it simply skips over `ReturnStmts` (it never walks them, it only sets `in_return` to `true`).

The patch would roughly be 

```patch
Index: crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs b/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs
--- a/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs	(revision 6ca384ae891a1f87b5429c2058e591a9d644d96b)
+++ b/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs	(date 1736158520791)
@@ -3,7 +3,7 @@
 use ruff_diagnostics::Diagnostic;
 use ruff_diagnostics::Violation;
 use ruff_macros::{derive_message_formats, ViolationMetadata};
-use ruff_python_ast::{self as ast, visitor::Visitor, Expr, Stmt};
+use ruff_python_ast::{self as ast, visitor, visitor::Visitor, Expr, Stmt};
 use ruff_text_size::TextRange;
 
 use crate::checkers::ast::Checker;
@@ -97,7 +97,7 @@
     }
 
     let mut visitor = ReturnInGeneratorVisitor::default();
-    ast::statement_visitor::StatementVisitor::visit_body(&mut visitor, &function_def.body);
+    visitor.visit_body(&function_def.body);
 
     if visitor.has_yield {
         if let Some(return_) = visitor.return_ {
@@ -117,146 +117,33 @@
 struct ReturnInGeneratorVisitor {
     return_: Option<TextRange>,
     has_yield: bool,
-    yield_expr_names: HashMap<String, BindState>,
-    yield_on_last_visit: bool,
 }
 
-impl ast::statement_visitor::StatementVisitor<'_> for ReturnInGeneratorVisitor {
+impl Visitor<'_> for ReturnInGeneratorVisitor {
     fn visit_stmt(&mut self, stmt: &Stmt) {
         match stmt {
-            Stmt::Expr(ast::StmtExpr { value, .. }) => match **value {
-                Expr::Yield(_) | Expr::YieldFrom(_) => {
-                    self.has_yield = true;
-                }
-                _ => {
-                    self.visit_expr(value);
-                }
-            },
             Stmt::FunctionDef(_) => {
                 // Do not recurse into nested functions; they're evaluated separately.
             }
-            Stmt::Assign(ast::StmtAssign { targets, value, .. }) => {
-                for target in targets {
-                    self.discover_yield_assignments(target, value);
-                }
-            }
-            Stmt::AnnAssign(ast::StmtAnnAssign {
-                target,
-                value: Some(value),
-                ..
-            }) => {
-                self.yield_on_last_visit = false;
-                self.visit_expr(value);
-                self.evaluate_target(target);
-            }
-            Stmt::Return(ast::StmtReturn {
-                value: Some(value),
-                range,
-            }) => {
-                if let Expr::Name(ast::ExprName { ref id, .. }) = **value {
-                    if !matches!(
-                        self.yield_expr_names.get(id.as_str()),
-                        Some(BindState::Reassigned) | None
-                    ) {
-                        return;
-                    }
-                }
-                self.return_ = Some(*range);
-            }
-            _ => ast::statement_visitor::walk_stmt(self, stmt),
-        }
-    }
-}
+            Stmt::Return(ast::StmtReturn { value, range }) => {
+                if value.is_some() {
+                    self.return_ = Some(*range);
+                }
+                return;
+            }
+            _ => ast::visitor::walk_stmt(self, stmt),
+        }
+    }
 
-impl Visitor<'_> for ReturnInGeneratorVisitor {
     fn visit_expr(&mut self, expr: &Expr) {
         match expr {
             Expr::Yield(_) | Expr::YieldFrom(_) => {
                 self.has_yield = true;
-                self.yield_on_last_visit = true;
             }
-            Expr::Lambda(_) | Expr::Call(_) => {}
+            Expr::Lambda(_) => {
+                // Don't traverse into call statements
+            }
             _ => ast::visitor::walk_expr(self, expr),
         }
     }
 }
-
-impl ReturnInGeneratorVisitor {
-    /// Determine if a target is bound to a yield or a yield from expression and,
-    /// if so, track that target
-    fn evaluate_target(&mut self, target: &Expr) {
-        if let Expr::Name(ast::ExprName { ref id, .. }) = *target {
-            if self.yield_on_last_visit {
-                match self.yield_expr_names.get(id.as_str()) {
-                    Some(BindState::Reassigned) => {}
-                    _ => {
-                        self.yield_expr_names
-                            .insert(id.to_string(), BindState::Stored);
-                    }
-                }
-            } else {
-                if let Some(BindState::Stored) = self.yield_expr_names.get(id.as_str()) {
-                    self.yield_expr_names
-                        .insert(id.to_string(), BindState::Reassigned);
-                }
-            }
-        }
-    }
-
-    /// Given a target and a value, track any identifiers that are bound to
-    /// yield or yield from expressions
-    fn discover_yield_assignments(&mut self, target: &Expr, value: &Expr) {
-        match target {
-            Expr::Name(_) => {
-                self.yield_on_last_visit = false;
-                self.visit_expr(value);
-                self.evaluate_target(target);
-            }
-            Expr::Tuple(ast::ExprTuple { elts: tar_elts, .. })
-            | Expr::List(ast::ExprList { elts: tar_elts, .. }) => match value {
-                Expr::Tuple(ast::ExprTuple { elts: val_elts, .. })
-                | Expr::List(ast::ExprList { elts: val_elts, .. })
-                | Expr::Set(ast::ExprSet { elts: val_elts, .. }) => {
-                    self.discover_yield_container_assignments(tar_elts, val_elts);
-                }
-                Expr::Yield(_) | Expr::YieldFrom(_) => {
-                    self.has_yield = true;
-                    self.yield_on_last_visit = true;
-                    self.evaluate_target(target);
-                }
-                _ => {}
-            },
-            _ => {}
-        }
-    }
-
-    fn discover_yield_container_assignments(&mut self, targets: &[Expr], values: &[Expr]) {
-        for (target, value) in targets.iter().zip(values) {
-            match target {
-                Expr::Tuple(ast::ExprTuple { elts: tar_elts, .. })
-                | Expr::List(ast::ExprList { elts: tar_elts, .. })
-                | Expr::Set(ast::ExprSet { elts: tar_elts, .. }) => {
-                    match value {
-                        Expr::Tuple(ast::ExprTuple { elts: val_elts, .. })
-                        | Expr::List(ast::ExprList { elts: val_elts, .. })
-                        | Expr::Set(ast::ExprSet { elts: val_elts, .. }) => {
-                            self.discover_yield_container_assignments(tar_elts, val_elts);
-                        }
-                        Expr::Yield(_) | Expr::YieldFrom(_) => {
-                            self.has_yield = true;
-                            self.yield_on_last_visit = true;
-                            self.evaluate_target(target);
-                        }
-                        _ => {}
-                    };
-                }
-                Expr::Name(_) => {
-                    self.yield_on_last_visit = false;
-                    self.visit_expr(value);
-                    self.evaluate_target(target);
-                }
-                _ => {}
-            }
-        }
-    }
-}
```

This patch goes beyond what bugbear does because bugbear only considers `yield` in expression statements. Doing exactly what bugbear wouldn't allow us to address the example raised in the issue. 

But I feel like I'm missing an important point because I also see that you added some special handling around `return` that goes beyond what the bugbear rule does. can you tell me more about the motivation for it?

Note: The bugbear rule also skips this rule when:

> ```
> # If the user explicitly wrote the 3-argument version of Generator as the
> # return annotation, they probably know what they were doing.
> ```

This could be a nice addition but doesn't need to be part of this PR.

---

_@MichaReiser reviewed on 2025-01-06 10:17_

---

_@kaspell reviewed on 2025-01-06 21:40_

---

_Review comment by @kaspell on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-06 21:40_

>But I feel like I'm missing an important point because I also see that you added some special handling around return that goes beyond what the bugbear rule does. can you tell me more about the motivation for it?

I think I might have got a bit carried away there by trying to cover as many scenarios as I could think of instead of trying to match the bugbear behavior (to be honest, it only now occurred to me that that might have been the goal!). The idea was that a function returning a variable can be valid or invalid depending on what the variable holds, e.g.

```
def f():
    x = yield from []
    return x
	
def g():
    x = 0
    yield from []
    return x
```

which would then necessitate some inference on what it is that the function is returning. I can totally drop that part if it's preferable to as closely as possible match what bugbear does!

Ty for the suggestions and the tip on Visitor! I'll adjust the approach based on these examples, with now a better idea of the constraints involved.

---

_@MichaReiser reviewed on 2025-01-07 09:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-07 09:32_

Oh I see, thanks. Covering assignments already means we go beyond what bugbear does. So just matching "bugbear" is a bit tricky 😅  


@AlexWaygood what's your take on this? 

---

_@kaspell reviewed on 2025-01-11 14:40_

---

_Review comment by @kaspell on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-11 14:40_

Alright, this time there should be subexpression traversal without otherwise straying too much from the bugbear implementation.

This seems to have required skipping assignment statements when searching for `yield` or `yield from` expressions. Bugbear doesn't check for them either but this means that, for example, one of the functions raised in the original issue:

```
def f():
    x = yield
    print(x)
    return 42
```

isn't currently caught. This is because, without tracking the actual return values, I don't think there's a way to catch this while at the same time respecting some of the existing example functions, such as:

```
def not_broken7():
    x = yield from []
    return x
```

Aside from this, function call expressions are also ignored (which bugbear does as well). Again, without some stronger machinery, existing example functions such as

```
def not_broken8():
    x = None

    def inner(ex):
        nonlocal x
        x = ex

    inner((yield from []))
    return x
```

would fail. Let me know if these sound like good tradeoffs, or if you noticed something that I missed!



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:126 on 2025-01-20 11:07_

Would you mind adding a comment why we aren't traversing into assignments

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-20 11:21_

These are great considerations! I'm somewhat inclined to leave the rule as is now, seeing that there are so many valid edge cases that I didn't consider, and re-visit the rule once we have a more precise type inference (to know if the returned value is a generator or not). 


---

_@MichaReiser reviewed on 2025-01-20 11:21_

---

_@kaspell reviewed on 2025-01-20 20:09_

---

_Review comment by @kaspell on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-20 20:09_

Sure! That makes sense. Previously I tried to find something like that but didn't come across anything.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-21 07:34_

Thanks for being so understanding and I'm sorry that I incorrectly marked the issue as "good first issue" which it clearly wasn't. 

Thanks again for your work. This was a great PR and you navigated the complexity well. 

---

_@MichaReiser reviewed on 2025-01-21 07:34_

---

_Closed by @MichaReiser on 2025-01-21 07:34_

---

_@kaspell reviewed on 2025-01-21 18:02_

---

_Review comment by @kaspell on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:124 on 2025-01-21 18:02_

Ty! Absolutely np, was fun to work on this regardless!

---
