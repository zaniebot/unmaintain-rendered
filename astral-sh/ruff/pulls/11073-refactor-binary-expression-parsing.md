```yaml
number: 11073
title: Refactor binary expression parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/refactor-binary-expr-parsing
created_at: 2024-04-21T15:38:34Z
updated_at: 2024-04-23T04:53:57Z
url: https://github.com/astral-sh/ruff/pull/11073
synced_at: 2026-01-12T15:55:34Z
```

# Refactor binary expression parsing

---

_@dhruvmanila_

## Summary

This PR refactors the binary expression parsing in a way to make it readable and easy to understand. It draws inspiration from the suggested edits in the linked messages in #10752.

### Changes

* Ability to get the precedence of an operator
	* From a boolean operator (`BinOp`) to `OperatorPrecedence`
	* From a binary operator (`Operator`) to `OperatorPrecedence`
	* No comparison operator because all of them have the same precedence
* Implement methods on `TokenKind` to convert it to an appropriate operator enum
	* Add `as_boolean_operator` which returns an `Option<BoolOp>`
	* Add `as_binary_operator` which returns an `Option<Operator>`
	* No `as_comparison_operator` because it requires lookahead and I'm not sure if `token.as_comparison_operator(peek)` is a good way to implement it
* Introduce `BinaryLikeOperator`
	* Constructed from two tokens using the methods from the second point
	* Add `precedence` method using the conversion methods mentioned in the first point
* Make most of the functions in `TokenKind` private to the module
* Use `self` instead of `&self` for `TokenKind` 

fixes: #11072

## Test Plan

Refer #11088 


---

_Label `internal` added by @dhruvmanila on 2024-04-21 15:38_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 15:38_

---

_Comment by @github-actions[bot] on 2024-04-21 16:04_

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

_Marked ready for review by @dhruvmanila on 2024-04-22 07:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-22 07:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:947 on 2024-04-22 08:08_

Nit: I would probably add a `kind` and `precedence` method to `BoolOp`. It improves readability and we don't realy need the `From` and `Into` infrastructure. 

Oh, or is this not possible (at least for precedence) because `OperatorPrecedence` is a parser and not an `AST` type.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2364 on 2024-04-22 08:11_

Nit: We use the term `BinaryLike` for types that cover Boolean, Comparison and Binary operations. I would then name this `BinaryLikeOperator` or `AnyBinaryLikeOperator`

---

_@MichaReiser approved on 2024-04-22 08:11_

---

_@dhruvmanila reviewed on 2024-04-23 04:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:947 on 2024-04-23 04:35_

Yeah, `OperatorPrecedence` is part of the parser crate. Also, the AST crate cannot depend on the parser crate as it'll create a cyclic dependency, so we can't define the `kind` on `BoolOp`.

---

_Merged by @dhruvmanila on 2024-04-23 04:42_

---

_Closed by @dhruvmanila on 2024-04-23 04:42_

---

_Branch deleted on 2024-04-23 04:42_

---
