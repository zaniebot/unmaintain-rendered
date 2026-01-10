```yaml
number: 11088
title: Refactor unary expression parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/refactor-unary-op
created_at: 2024-04-22T07:01:51Z
updated_at: 2024-04-23T05:04:52Z
url: https://github.com/astral-sh/ruff/pull/11088
synced_at: 2026-01-10T22:37:01Z
```

# Refactor unary expression parsing

---

_Pull request opened by @dhruvmanila on 2024-04-22 07:01_

## Summary

This PR refactors unary expression parsing with the following changes:
* Ability to get `OperatorPrecedence` from a unary operator (`UnaryOp`)
* Implement methods on `TokenKind`
	* Add `as_unary_operator` which returns an `Option<UnaryOp>`
	* Add `as_unary_arithmetic_operator` which returns an `Option<UnaryOp>` (used for pattern parsing)
	* Rename `is_unary` to `is_unary_arithmetic_operator` (used in the linter)

resolves: #10752 

## Test Plan

Verify that the existing test cases pass, no ecosystem changes, run the Python based fuzzer on 3000 random inputs and run it on dozens of open-source repositories.


---

_Label `internal` added by @dhruvmanila on 2024-04-22 07:01_

---

_Label `parser` added by @dhruvmanila on 2024-04-22 07:01_

---

_Marked ready for review by @dhruvmanila on 2024-04-22 07:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-22 07:06_

---

_Comment by @github-actions[bot] on 2024-04-22 07:19_

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

_@MichaReiser approved on 2024-04-22 08:13_

---

_Merged by @dhruvmanila on 2024-04-23 04:55_

---

_Closed by @dhruvmanila on 2024-04-23 04:55_

---

_Branch deleted on 2024-04-23 04:55_

---
