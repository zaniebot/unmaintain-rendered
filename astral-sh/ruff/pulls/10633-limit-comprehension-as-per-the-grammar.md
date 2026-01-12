```yaml
number: 10633
title: Limit comprehension as per the grammar
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/limit-comp
created_at: 2024-03-27T19:52:33Z
updated_at: 2024-03-29T08:37:24Z
url: https://github.com/astral-sh/ruff/pull/10633
synced_at: 2026-01-12T15:55:32Z
```

# Limit comprehension as per the grammar

---

_@dhruvmanila_

## Summary

This PR updates the comprehension parsing logic to validate the parsed expressions as per the grammar. The validations are:
1. `for` target, it has same semantics as an assignment target (`x for target in iter`)
2. Iter expression (`x for x in iter`)
3. If expression (`x for x in iter if expr`)

## Test Plan

The tests will be added in the PRs for specific comprehension types like lists, sets, etc.


---

_Label `parser` added by @dhruvmanila on 2024-03-27 19:52_

---

_@dhruvmanila reviewed on 2024-03-28 06:49_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1588 on 2024-03-28 06:49_

This method is just being moved from the bottom.

---

_@dhruvmanila reviewed on 2024-03-28 06:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1656 on 2024-03-28 06:50_

Parenthesized starred expression is also an error but will be reported by `parse_parenthesized_expression` in #10641 

---

_@dhruvmanila reviewed on 2024-03-28 06:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1672 on 2024-03-28 06:50_

I might just add a `ExprKind` with additional error types like `UnparenthesizedExpr(ExprKind)` later

---

_Marked ready for review by @dhruvmanila on 2024-03-28 06:53_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-28 06:53_

---

_@MichaReiser approved on 2024-03-28 08:14_

---

_Merged by @dhruvmanila on 2024-03-29 08:19_

---

_Closed by @dhruvmanila on 2024-03-29 08:19_

---

_Branch deleted on 2024-03-29 08:19_

---

_Comment by @github-actions[bot] on 2024-03-29 08:37_

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
