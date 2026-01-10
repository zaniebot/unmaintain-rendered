```yaml
number: 10657
title: "Limit `bitwise_or` precedence for starred expression with error"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/star-expr
created_at: 2024-03-29T14:21:23Z
updated_at: 2024-04-03T07:49:54Z
url: https://github.com/astral-sh/ruff/pull/10657
synced_at: 2026-01-10T22:47:02Z
```

# Limit `bitwise_or` precedence for starred expression with error

---

_Pull request opened by @dhruvmanila on 2024-03-29 14:21_

## Summary

This PR updates handling the error handling for `bitwise_or` grammar rule.

**tldr,**
* Remove `parse_expression_with_bitwise_or_precedence`
* Add a new `validate_expression_with_bitwise_or_precedence`
* Use the validation method wherever the precedence is `bitwise_or`

Previously, there was a `parse_expression_with_bitwise_or_precedence` parser method which would parse the expression with `bitwise_or` precedence using the Pratt Parsing technique. There are two things to understand with the current implementation of Pratt parser:

1. The parsing of LHS expression doesn't take into account the previous precedence. This is important to disallow expression as per the precedence. This has been fixed in #10679.
2. The precedence parsing **stops** when the precedence of the current operator is less than the precedence of the previous operator. Note that it doesn't report an error.

With (2) and `yield` expression, we are in a bit of a pickle. So, `yield` is both a statement and an expression. The grammar has `yield_stmt` and `yield_expr`. But, our AST doesn't have a node to represent the yield statement. This means the parser always parse it using the expression method. This creates a problem. For reference, the grammar for yield statement and expression is:

```
yield_stmt: yield_expr

yield_expr:
    | 'yield' 'from' expression 
    | 'yield' [star_expressions]

star_expression:
	| '*' bitwise_or
	| expression
```

And, for some more context, the different between `expression` and `star_expression` is that for the later, the precedence of a starred expression is bitwise OR. The `expression` grammar doesn't allow starred expression but we parse it and then later report an error.

Now, for the following example:

```py
yield *x and y
```

How should the AST look like? Should it be `(yield *x) and y` or `yield *(x and y)`? This is a yield statement and so it consumes the entire expression. If not, this would become a boolean expression with LHS being `yield *x` and RHS being `y`.

Another problem here is about error reporting. The above is not a valid syntax as starred comparison expression isn't allowed in that context. There are two solutions to this:
1. Keep the `parse_expression_with_bitwise_or_precedence` and use the current token to validate the expression. Remember that this happens because the Pratt Parser stops at the `and` token due to the precedence. The AST would still output a comparison expression.
2. Remove `parse_expression_with_bitwise_or_precedence`, parse **all** starred expression with the highest precedence, and add a validation step for wherever it is required the precedence to be bitwise OR. This would keep the AST as a yield expression as is in the code. **This is the chosen solution in this PR.**

This also helps in keeping our AST match the program whereas previously, as the Pratt Parsing would stop, the final AST would look a bit different. This can be observed with the updated AST snapshots in this PR.

## Test Plan

Update the snapshots with the new error handling.


---

_Label `parser` added by @dhruvmanila on 2024-03-29 14:21_

---

_Comment by @github-actions[bot] on 2024-03-29 14:41_

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

_Marked ready for review by @dhruvmanila on 2024-04-01 05:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-01 05:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1555 on 2024-04-02 13:45_

Would it make sense to keep a `parse_expression_with_bitwise_or_precedence` that calls into `parse_conditional_expression_or_higher` and calls into the validation to avoid the repetition of the validation code?

---

_@MichaReiser approved on 2024-04-02 13:49_

Nit: I think an alternative could have been to add a `PARENTHESIZED` to the `ParserContext` and only parse `yield` expression if that's true (or have an explicit `ExprContext` that is passed around and stores whether the currently parsed expression is parenthesized and, thus, allows yield expressions)

But that wouldn't have given us such nice error recovery :)

---

_@dhruvmanila reviewed on 2024-04-03 07:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1555 on 2024-04-03 07:31_

Yeah, I like that

---

_Merged by @dhruvmanila on 2024-04-03 07:32_

---

_Closed by @dhruvmanila on 2024-04-03 07:32_

---

_Branch deleted on 2024-04-03 07:32_

---
