```yaml
number: 10623
title: Implement rules around star expressions with different precedence
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/star-expression-list
created_at: 2024-03-27T08:39:37Z
updated_at: 2024-03-29T07:48:17Z
url: https://github.com/astral-sh/ruff/pull/10623
synced_at: 2026-01-12T15:55:32Z
```

# Implement rules around star expressions with different precedence

---

_@dhruvmanila_

## Summary

This PR adds implementations around the following grammar rules to support the other expression and statement parsing. This PR doesn't update the related expressions and statements as they will be done in their own PRs.

* `star_expressions`
* `star_expression`
* `'*' bitwise_or`

### Problem

There are two grammar entries around star expressions:

```
star_expression:
    | '*' bitwise_or 
    | expression

starred_expression:
    '*' expression
```

The main difference is the precedence at which the expression should be parsed if a `*` token is encountered. Currently, we parse both rules at the same precedence which makes the parser more lenient. There are various expressions and statements which uses either one of the above rules.

### Solutions

#### Current solution

Add the following new methods to the parser:
1. `parse_star_expression_list` which would be similar to `parse_expression_list` and would match the `star_expressions` grammar rule
2. `parse_star_expression_or_higher` which would match the `star_expression` or `star_named_expression` grammar rule
3. `parse_expression_with_bitwise_or_precedence` which would match the `bitwise_or` grammar rule

Here, we could possibly merge the `parse_expression_list` and `parse_star_expression_list` into a single function as it only differs in the method it delegates the parsing to. The parameter could be called `allow_star_expression`. It could be confusing because even if it’s false, we would allow starred expression but at a higher binding power.

This solution closely resembles the actual grammar and is easier to follow in the parser code. It doesn't require a lot of verification by the caller as the precedence parser makes sure to not allow that. 

#### Other solution

Add a new `verify_bitwise_or_precedence` method which reports an error if the grammar expected a starred expression with `bitwise_or` precedence:

```rs
pub(super) fn verify_star_expressions(
    &mut self,
    expr: &Expr,
    allow_named_expression: AllowNamedExpression,
) {
    match expr {
        Expr::Tuple(ast::ExprTuple { elts, .. }) => {
            for expr in elts {
                self.verify_star_expressions(expr, allow_named_expression);
            }
        }
        Expr::Starred(ast::ExprStarred { value, .. }) => {
            let expr_kind = match &**value {
                Expr::Compare(_) => "comparison",
                Expr::UnaryOp(ast::ExprUnaryOp {
                    op: ast::UnaryOp::Not,
                    ..
                }) => "unary `not`",
                Expr::BoolOp(_) => "boolean",
                Expr::If(_) => "`if-else`",
                Expr::Lambda(_) => "lambda",
                Expr::Named(_) if allow_named_expression.is_no() => "named",
                _ => return,
            };

            self.add_error(
                ParseErrorType::OtherError(format!(
                    "{expr_kind} expression cannot be used here",
                )),
                expr.range(),
            );
        }
        _ => {}
    }
}
```

For this solution, the parser will need to verify the precedence of a starred expression wherever grammar expected the `star_expressions`, `star_expression` or `star_named_expression` rule.

We could possibly adopt this solution for better error recovery but I think for now having a strict parser is better suited because we don't really know what complete error recovery looks like.

### Follow-up

There's a related follow-up issue which I'm working on and is related to starred expressions.
The starred expression is only allowed in certain expressions and context which the parser needs to make sure and report an error if found otherwise.

## Test Plan

Tested this locally by changing various expression and statement to use these methods. As mentioned they will be in their own PR.


---

_Label `parser` added by @dhruvmanila on 2024-03-27 08:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-27 08:39_

---

_Converted to draft by @dhruvmanila on 2024-03-27 08:57_

---

_Comment by @dhruvmanila on 2024-03-27 08:57_

(Converting to draft because I think it might be better to use the second solution as described in the PR description.)

---

_Comment by @github-actions[bot] on 2024-03-27 08:58_

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

_Marked ready for review by @dhruvmanila on 2024-03-27 09:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:86 on 2024-03-27 10:10_

Nit: i find the `postfix` list a bit confusing because we aren't parsing a list (and the term is overloaded in the `Parser` context where we have `parse_list` and `parse_list_expression`) but I don't have a better name recommendation. 

---

_@MichaReiser approved on 2024-03-27 10:10_

---

_Comment by @dhruvmanila on 2024-03-29 07:47_

For future reference, this change is going to be updated to use the "other" solution mentioned in the PR description. Not all will be reverted but instead of having a `parse_expression_with_bitwise_or_precedence`, we'll allow the starred expression to be parsed with the highest precedence and limit it later by checking if it's allowed as per "bitwise or" precedence and report an error.

I don't want to add the change here as it'll require updating all of the PRs in the stack. I'll link the PR when it's opened.

---

_Merged by @dhruvmanila on 2024-03-29 07:48_

---

_Closed by @dhruvmanila on 2024-03-29 07:48_

---

_Branch deleted on 2024-03-29 07:48_

---
