```yaml
number: 12067
title: Avoid consuming newline for unterminated string
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/unterminated-string
created_at: 2024-06-27T10:20:38Z
updated_at: 2024-06-27T11:32:51Z
url: https://github.com/astral-sh/ruff/pull/12067
synced_at: 2026-01-12T15:55:40Z
```

# Avoid consuming newline for unterminated string

---

_@dhruvmanila_

## Summary

This PR fixes the lexer logic to **not** consume the newline character for an unterminated string literal.

Currently, the lexer would consume it to be part of the string itself but that would be bad for recovery because then the lexer wouldn't emit the newline token ever. This PR fixes that to avoid consuming the newline character in that case.

This was discovered during https://github.com/astral-sh/ruff/pull/12060.

## Test Plan

Update the snapshots and validate them.

---

_Label `parser` added by @dhruvmanila on 2024-06-27 10:31_

---

_Marked ready for review by @dhruvmanila on 2024-06-27 10:31_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-27 10:31_

---

_@dhruvmanila reviewed on 2024-06-27 10:33_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@implicitly_concatenated_unterminated_string.py.snap`:171 on 2024-06-27 10:33_

The reason for this "Expected a statement" error is because the current token is `Newline` which doesn't start a module statement nor ends a list of module statements.

---

_@dhruvmanila reviewed on 2024-06-27 10:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@implicitly_concatenated_unterminated_string.py.snap`:171 on 2024-06-27 10:36_

I think this might get fixed if we start emitting `String` tokens instead of `Unknown` token because currently the parser thinks that the statement ended after `'hello'` but then it encountered an `Unknown` token (for unterminated string) and a `Newline` token both of which doesn't start or end a statement.

---

_Comment by @github-actions[bot] on 2024-06-27 10:43_

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

_@MichaReiser approved on 2024-06-27 11:22_

Makes sense

---

_Merged by @dhruvmanila on 2024-06-27 11:32_

---

_Closed by @dhruvmanila on 2024-06-27 11:32_

---

_Branch deleted on 2024-06-27 11:32_

---
