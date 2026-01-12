```yaml
number: 10556
title: Add tests for dictionary expression parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-dict-parsing
created_at: 2024-03-25T03:18:09Z
updated_at: 2024-03-29T08:26:17Z
url: https://github.com/astral-sh/ruff/pull/10556
synced_at: 2026-01-12T15:55:32Z
```

# Add tests for dictionary expression parsing

---

_@dhruvmanila_

## Summary

This PR updates the existing valid test case for dictionary expression and adds some invalid ones to check how the parser is recovering.

The goal is mainly to check if the dictionary parsing is correct or not.


---

_Label `parser` added by @dhruvmanila on 2024-03-25 03:18_

---

_Label `testing` added by @dhruvmanila on 2024-03-25 03:18_

---

_@dhruvmanila reviewed on 2024-03-27 09:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/dict/recover.py`:27 on 2024-03-27 09:54_

This needs to be solved holistically similar to how it was done in star pattern parsing.

---

_@dhruvmanila reviewed on 2024-03-27 09:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1325 on 2024-03-27 09:55_

Even parenthesized starred expression isn't allowed but that check will be done by `parse_parenthesized_expression` and not here.

---

_@dhruvmanila reviewed on 2024-03-27 09:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1518 on 2024-03-27 09:56_

The idea to disallow starred expression in certain context would be to pass `AllowStarExpression` as an argument to `parse_conditional_expression_or_higher` and it'll report an error is it's not allowed. This way it's isolated to a single place. It'll be done in a follow-up PR.

---

_Marked ready for review by @dhruvmanila on 2024-03-27 09:58_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-27 09:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__dict__double_star.py.snap`:505 on 2024-03-27 10:26_

I'm a bit surprised by the recovery here. Should it not recognize that `if` is a valid expression start and parse the expression instead? 


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__dict__double_star.py.snap`:631 on 2024-03-27 10:28_

This is for the future. We could in theory parse this as a binary expression with a missing left side.

---

_@MichaReiser approved on 2024-03-27 10:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/expressions/dict/missing_closing_brace_0.py`:1 on 2024-03-27 10:30_

I assume the reason why there's no test for an empty dictionary is because that's always parsed as an empty set?

---

_@MichaReiser reviewed on 2024-03-27 10:30_

---

_@dhruvmanila reviewed on 2024-03-28 09:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__dict__double_star.py.snap`:505 on 2024-03-28 09:42_

The minimum precedence for an expression after `**` in a dictionary is bitwise or (`|`) and `if` expressions are lower than that. The parser won't parse the `if ...` because the grammar doesn't allow it. This is one of the areas which can possibly be changed when working on error recovery.

---

_@dhruvmanila reviewed on 2024-03-28 09:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/dict/missing_closing_brace_0.py`:1 on 2024-03-28 09:43_

Yes, unless the parser sees a `:`, it assumes that we're parsing a set.

---

_@dhruvmanila reviewed on 2024-03-29 08:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__dict__double_star.py.snap`:631 on 2024-03-29 08:17_

The parser will be able to recover well with the star expression update I will be making in a follow-up PR. The grammar here is similar to star expression (`** bitwise_or`) but we'll allow the parser to be lenient and report an error.

---

_Merged by @dhruvmanila on 2024-03-29 08:17_

---

_Closed by @dhruvmanila on 2024-03-29 08:17_

---

_Branch deleted on 2024-03-29 08:17_

---

_Comment by @github-actions[bot] on 2024-03-29 08:26_

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
