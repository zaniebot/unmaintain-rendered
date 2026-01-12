```yaml
number: 11066
title: "Use `BoolOp` parameter in boolean expression parsing"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
draft: true
base: dhruv/precedence-parsing-refactor
head: dhruv/refactor-bool-op
created_at: 2024-04-21T06:03:55Z
updated_at: 2024-04-21T15:55:08Z
url: https://github.com/astral-sh/ruff/pull/11066
synced_at: 2026-01-12T15:55:34Z
```

# Use `BoolOp` parameter in boolean expression parsing

---

_@dhruvmanila_

## Summary

Part of #10752 

This PR updates boolean expression parsing to take in the `BoolOp` as a parameter. This avoids the `unwrap` call when converting `TokenKind` to `BoolOp`. Instead, we use `Option<BoolOp>` and only call the parsing function if the parser is at a boolean operator.

## Test Plan

Make sure the boolean expression parsing logic is still correct by running the test suite, fuzz testing it and running it against a dozen or so open-source repositories.


---

_Label `internal` added by @dhruvmanila on 2024-04-21 06:03_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 06:03_

---

_Comment by @github-actions[bot] on 2024-04-21 07:29_

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

_Comment by @dhruvmanila on 2024-04-21 15:55_

Closed in favor of #11073 

---

_Closed by @dhruvmanila on 2024-04-21 15:55_

---
