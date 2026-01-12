```yaml
number: 16162
title: "[internal] Move Linter `OperatorPrecedence` into `ruff_python_ast` crate"
type: pull_request
state: merged
author: junhsonjb
labels:
  - internal
assignees: []
merged: true
base: main
head: jjb-organize-operator-precedence
created_at: 2025-02-14T14:26:21Z
updated_at: 2025-03-18T05:34:05Z
url: https://github.com/astral-sh/ruff/pull/16162
synced_at: 2026-01-12T15:55:53Z
```

# [internal] Move Linter `OperatorPrecedence` into `ruff_python_ast` crate

---

_@junhsonjb_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This change begins to resolve #16071 by moving the `OperatorPrecedence` structs from the `ruff_python_linter` crate into `ruff_python_ast`. This PR also implements `precedence()` methods on the `Expr` and `ExprRef` enums.

## Test Plan

<!-- How was it tested? -->

Since this change mainly shifts existing logic, I didn't add any additional tests. Existing tests do pass.

---

_Comment by @github-actions[bot] on 2025-02-14 14:53_

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

_Label `internal` added by @MichaReiser on 2025-02-14 14:54_

---

_@MichaReiser approved on 2025-02-14 14:54_

Sweet

---

_Merged by @MichaReiser on 2025-02-14 14:55_

---

_Closed by @MichaReiser on 2025-02-14 14:55_

---

_@dhruvmanila reviewed on 2025-03-18 05:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/operator_precedence.rs`:162 on 2025-03-18 05:34_

I don't think both `BitXor` and `BitOr` are at the same precedence level (https://docs.python.org/3/reference/expressions.html#operator-precedence).

---
