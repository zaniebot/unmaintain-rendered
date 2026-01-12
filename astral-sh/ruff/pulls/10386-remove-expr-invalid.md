```yaml
number: 10386
title: "Remove `Expr::Invalid`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/remove-invalid-expr
created_at: 2024-03-13T14:57:50Z
updated_at: 2024-03-18T14:08:11Z
url: https://github.com/astral-sh/ruff/pull/10386
synced_at: 2026-01-12T15:55:32Z
```

# Remove `Expr::Invalid`

---

_@dhruvmanila_

## Summary

This PR removes the `Expr::Invalid` variant from the AST. Instead, we'll try to retain as much valid information as possible and use an empty `Expr::Name` with `ExprContext::Invalid` as a replacement.

## Test Plan

- [x] All tests pass
- [x] No performance regression


---

_Label `parser` added by @dhruvmanila on 2024-03-13 14:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/tests/parser.rs`:874 on 2024-03-14 03:18_

This is actually an invalid syntax for which we now raise an error.

```console
$ python3.13 -m ast parser/_.py
Traceback (most recent call last):
  File "parser/_.py", line 2
    case 1 + 1:
             ^
SyntaxError: imaginary number required in complex literal
```

---

_@dhruvmanila reviewed on 2024-03-14 03:18_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1173 on 2024-03-14 03:23_

The rules are same. I've referenced the CPython parser.

---

_@dhruvmanila reviewed on 2024-03-14 03:23_

---

_@dhruvmanila reviewed on 2024-03-14 03:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:136 on 2024-03-14 03:24_

If the current token is either a `+` or `-`, then the LHS can only be either a number or unary op with a number as stated in the grammar.

---

_@dhruvmanila reviewed on 2024-03-14 03:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:166 on 2024-03-14 03:25_

In a pattern `+` / `-` is always used for complex numbers. There can't be binary operations in a pattern.

---

_Marked ready for review by @dhruvmanila on 2024-03-14 03:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-14 03:32_

---

_Comment by @github-actions[bot] on 2024-03-14 03:56_

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

_Merged by @dhruvmanila on 2024-03-14 04:05_

---

_Closed by @dhruvmanila on 2024-03-14 04:05_

---

_Branch deleted on 2024-03-14 04:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:155 on 2024-03-18 13:24_

What happens with the parsed out `lhs` in this case? Do we just drop it? 

---

_@MichaReiser reviewed on 2024-03-18 13:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:186 on 2024-03-18 13:24_

Same as for `lhs`. Do we drop the parsed `rhs_pattern` in case it is invalid? 

---

_@MichaReiser reviewed on 2024-03-18 13:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:256 on 2024-03-18 13:27_

We shouldn't use `ExprName` to represent any invalid expression. The idea of `ExprName` with `ctx` `Invalid` is to only be used in situations where the parser must return an expression but the parser isn't positioned at an expression. This is different here. We did parse a pattern and we shouldn just convert this pattern to be a name. Doing so can lead to problems downstream, e.g. the formatter would replace the pattern (even when invalid) in the source with an empty name, deleting code! 



---

_@MichaReiser reviewed on 2024-03-18 13:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:653 on 2024-03-18 13:28_

Same as above. I don't think that converting any pattern to a name is the right thing to do here. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:667 on 2024-03-18 13:28_

Same, I don't think we should convert arbitrary expressions or patterns to `ExprName` with the `ctx` `Invalid`. That can lead to issues downstream

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:546 on 2024-03-18 13:29_

Creating an empty `name` with context `Invalid` is valid here but we shouldn't consume the token in that case. 

---

_@MichaReiser reviewed on 2024-03-18 13:31_

I think we need to revisit the cases where we use `Expr::Name` with `ctx` `Invalid`. We shouldn't use it to represent arbitrary expressions or patterns. We should only use it to represent tokens that could be names (keywords) or in situations where the parser must return an expression but it isn't positioned at an expression (in which case we create an empty name).

---

_@dhruvmanila reviewed on 2024-03-18 13:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:256 on 2024-03-18 13:54_

I see. Thanks for pointing it out. Would this mean that the parser should create an expression node which matches the pattern and use that instead? For example, `Pattern::Singleton` can be converted to `Expr::BooleanLiteral` / `Expr::NoneLiteral`.

---

_@MichaReiser reviewed on 2024-03-18 13:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/pattern.rs`:256 on 2024-03-18 13:58_

I think that would be preferable. I would need to look into the specific recovery case here (a few tests would help) to fully understand how the parser should recover.  

---

_@dhruvmanila reviewed on 2024-03-18 14:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:256 on 2024-03-18 14:08_

Yes, my plan for the week is to focus on testing and fixing / updating the logic as required. I'll do the match statement first then to get this fixed.

---
