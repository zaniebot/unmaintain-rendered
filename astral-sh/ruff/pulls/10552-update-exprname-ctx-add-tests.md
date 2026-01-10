```yaml
number: 10552
title: "Update `ExprName` ctx, add tests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/expr-name
created_at: 2024-03-25T03:12:00Z
updated_at: 2024-03-25T12:38:50Z
url: https://github.com/astral-sh/ruff/pull/10552
synced_at: 2026-01-10T22:47:02Z
```

# Update `ExprName` ctx, add tests

---

_Pull request opened by @dhruvmanila on 2024-03-25 03:12_

## Summary

This PR does the following:
1. Update name parsing to use `ExprContext::Invalid` if identifier is invalid
2. Add valid tests for name and a few atoms
3. Update documentation for name, identifier and atom parsing

## Test Plan

Add tests, then update and review the snapshots.


---

_Label `parser` added by @dhruvmanila on 2024-03-25 03:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-25 03:12_

---

_Comment by @github-actions[bot] on 2024-03-25 03:32_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:260 on 2024-03-25 08:40_

Thank you so much for taking the time to add these! I find these super helpful.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:262 on 2024-03-25 08:41_

Nit: Should we change `parse_identifier` to return a `Result` (`Result<Identifier, Identifier>`) to make the "invalid" check more explicit and require it for all call-sites (doesn't need to be part of this PR). 

Would that allow removing `is_valid` from` Identifier`?

---

_@MichaReiser approved on 2024-03-25 08:42_

---

_@dhruvmanila reviewed on 2024-03-25 12:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:262 on 2024-03-25 12:36_

Using `Result` is a good idea to enforce this from type perspective but I don't think it'll allow us to remove the `is_valid`. The reason being that certain nodes store the `Identifier` directly and not convert it to `ExprName`. For those nodes, it'll be useful to check if the identifier is valid or not.

---

_Merged by @dhruvmanila on 2024-03-25 12:38_

---

_Closed by @dhruvmanila on 2024-03-25 12:38_

---

_Branch deleted on 2024-03-25 12:38_

---
