```yaml
number: 11068
title: "Use `CmpOp` parameter in compare expression parsing"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
draft: true
base: dhruv/refactor-bool-op
head: dhruv/refactor-compare-op
created_at: 2024-04-21T07:12:44Z
updated_at: 2024-04-21T15:55:16Z
url: https://github.com/astral-sh/ruff/pull/11068
synced_at: 2026-01-10T22:37:01Z
```

# Use `CmpOp` parameter in compare expression parsing

---

_Pull request opened by @dhruvmanila on 2024-04-21 07:12_

## Summary

Part of #10752 

This PR updates compare expression parsing to take in the `CmpOp` as a parameter.

## Test Plan

Make sure the compare expression parsing logic is still correct by running the test suite, fuzz testing it and running it against a dozen or so open-source repositories.


---

_Label `internal` added by @dhruvmanila on 2024-04-21 07:12_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 07:12_

---

_Comment by @github-actions[bot] on 2024-04-21 07:52_

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
