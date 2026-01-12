```yaml
number: 10809
title: Avoid error handling duplication for starred, yield, lambda expressions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser
head: dhruv/deduplicate-expr-error-handling
created_at: 2024-04-07T02:23:46Z
updated_at: 2024-04-09T09:07:49Z
url: https://github.com/astral-sh/ruff/pull/10809
synced_at: 2026-01-12T15:55:33Z
```

# Avoid error handling duplication for starred, yield, lambda expressions

---

_@dhruvmanila_

## Summary

This PR updates the error handling logic for certain expressions in a way to either perform it automatically or provide an option for the user. The expression in discussion here are `lambda`, starred and `yield` expression.

### Problem

The current parser allows these expressions at arbitrary context. This is because the mentioned expressions are parsed using `parse_lhs_expression` which is part of other higher level grammar rules. This means that the caller needs to validate the parsed expression and report an error if it isn't allowed in that context. This can get quite cumbersome to do so as it needs to be done for all of the call sites for following methods:

1. `parse_expression_list`: 14 references
2. `parse_star_expression_list`: 4 references
3. `parse_star_expression_or_higher`: 8 references
4. `parse_named_expression_or_higher`: 10 references
5. `parse_conditional_expression_or_higher`: 25 references
6. `parse_simple_expression`: 4 references

The numbers corresponding to the methods are the number of references as of today. This list is also in the correct hierarchy of grammar precedence. For example, `parse_expression_list` calls into `parse_conditional_expression_or_higher` but not the other way around.

### Solution

We'll take the above expression one at a time to understand the solution:

#### Lambda expression

Lambda expressions are only allowed in `expression` grammar rule which corresponds to `parse_conditional_expression_or_higher`. This means that this expression is only allowed when using either of the following functions:

1. `parse_expression_list`
2. `parse_star_expression_list`
3. `parse_star_expression_or_higher`
4. `parse_named_expression_or_higher`
5. `parse_conditional_expression_or_higher`

The solution is to move the error handling in `parse_simple_expression` and parameterize it where any of the above listed function would always use `AllowLambdaExpression::Yes`.

#### Starred expression

There are two grammar rules related to starred expression:
1. `star_expression` which corresponds to `parse_star_expression_or_higher`
2. `starred_expression` which is parsed in LHS parsing

Remember that LHS parsing isn't accessed directly but only via any of the above listed functions in the problem section. Now, starred expressions are allowed in a lot of places but sometimes in a limited capacity. For example, an assignment target can have a starred expression but only if it is a name node (`*x`).

The solution here is to adopt the one used in star pattern matching which is to use a parameter. The following functions are parameterized:
1. `parse_expression_list`
2. `parse_named_expression_or_higher`
3. `parse_conditional_expression_or_higher`

Now, `parse_star_expression_list` and `parse_star_expression_or_higher` aren't parameterized because they handle the `star_expression` grammar which means that the caller wants to parse a starred expression but with a limited precedence.

#### Yield expression

Yield expressions are only allowed in the following context:
1. Top level as yield statement
2. Parenthesized
3. F-string expression
4. Assignment (including annotated and augmented) value

We could parameterize it similar to starred expression but that seems like a waste given the limited number of locations they're allowed.

The solution is to add a `parse_yield_expression_or_else` method which parses a yield expression if the parser is at `yield` token or else calls the given method to parse the expression. The call site would like:

```rs
// (yield_expr | named_expression)
self.try_parse_yield_expression()
  .unwrap_or_else(|| self.parse_named_expression_or_higher())

// (yield_expr | star_expressions)
self.try_parse_yield_expression()
  .unwrap_or_else(|| self.parse_star_expression_list())
```

An added benefit for this is that the call site looks exactly like the grammar.

## Review

* The reviewer would mainly just look at the de-duplication logic.
* The reviewer doesn't really need to verify the call sites as they're verified by existing test cases. For nodes which aren't yet tested, they will be done so in their own PR.

## Test Plan

Run existing test cases and verify the snapshot updates.

Additional test cases will be added when working on specific nodes.


---

_Label `internal` added by @dhruvmanila on 2024-04-07 02:23_

---

_Comment by @github-actions[bot] on 2024-04-07 04:13_

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

_Marked ready for review by @dhruvmanila on 2024-04-07 11:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 11:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__if__recover.py.snap`:312 on 2024-04-08 09:08_

You quote `lambda` in this error message but `yield` isn't quoted in the error message below. I think we should either always or never quote the expression name.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1971 on 2024-04-08 09:21_

Nit: It seems that `starred` expressions are ever only allowed when lambda expressions are allowed. We could consider having a separate method for lambda/no-lambda or design the argument so that it only ever supports Lambda yes with starred yes

---

_@MichaReiser approved on 2024-04-08 09:25_

---

_@dhruvmanila reviewed on 2024-04-09 08:33_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1971 on 2024-04-09 08:33_

Actually, I think we can remove the `AllowLambdaExpression` because it's only allowed in `parse_conditional_expression_or_higher`. So, we can have a dedicated branch for it and always throw an error if we see it at a later point. This would be similar to yield expression but without the `parse_yield_expression_or_higher` method.

```diff
diff --git a/crates/ruff_python_parser/src/parser/expression.rs b/crates/ruff_python_parser/src/parser/expression.rs
index cedc5a7da..8a1a026b5 100644
--- a/crates/ruff_python_parser/src/parser/expression.rs
+++ b/crates/ruff_python_parser/src/parser/expression.rs
@@ -208,14 +208,18 @@ impl<'src> Parser<'src> {
         &mut self,
         allow_starred_expression: AllowStarredExpression,
     ) -> ParsedExpr {
-        let start = self.node_start();
-        let parsed_expr =
-            self.parse_simple_expression(AllowLambdaExpression::Yes, allow_starred_expression);
-
-        if self.at(TokenKind::If) {
-            Expr::If(self.parse_if_expression(parsed_expr.expr, start)).into()
+        if self.at(TokenKind::Lambda) {
+            Expr::Lambda(self.parse_lambda_expr()).into()
         } else {
-            parsed_expr
+            let start = self.node_start();
+            let parsed_expr =
+                self.parse_simple_expression(AllowLambdaExpression::Yes, allow_starred_expression);
+
+            if self.at(TokenKind::If) {
+                Expr::If(self.parse_if_expression(parsed_expr.expr, start)).into()
+            } else {
+                parsed_expr
+            }
         }
     }
 
@@ -440,9 +444,7 @@ impl<'src> Parser<'src> {
             }
             TokenKind::Lambda => {
                 let lambda_expr = self.parse_lambda_expr();
-                if previous_precedence > Precedence::Initial {
-                    self.add_error(ParseErrorType::InvalidLambdaExpressionUsage, &lambda_expr);
-                }
+                self.add_error(ParseErrorType::InvalidLambdaExpressionUsage, &lambda_expr);
                 Expr::Lambda(lambda_expr).into()
             }
             TokenKind::Yield => {

```

---

_@dhruvmanila reviewed on 2024-04-09 08:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__if__recover.py.snap`:312 on 2024-04-09 08:47_

There are other inconsistencies with error messages, I'll club this with them in a single PR. Thanks for noticing.

---

_Merged by @dhruvmanila on 2024-04-09 08:48_

---

_Closed by @dhruvmanila on 2024-04-09 08:48_

---

_Branch deleted on 2024-04-09 08:48_

---
