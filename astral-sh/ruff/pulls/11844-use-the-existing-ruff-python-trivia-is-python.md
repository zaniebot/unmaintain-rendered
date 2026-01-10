```yaml
number: 11844
title: "Use the existing `ruff_python_trivia::is_python_whitespace` function"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/whitespace
created_at: 2024-06-12T05:55:39Z
updated_at: 2024-06-12T06:15:08Z
url: https://github.com/astral-sh/ruff/pull/11844
synced_at: 2026-01-10T21:56:00Z
```

# Use the existing `ruff_python_trivia::is_python_whitespace` function

---

_Pull request opened by @dhruvmanila on 2024-06-12 05:55_

## Summary

This PR re-uses the `ruff_python_trivia::is_python_whitespace` in the lexer instead of defining its own. This was mainly to avoid circular dependency which was resolved in #11261.


---

_Label `internal` added by @dhruvmanila on 2024-06-12 05:55_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-12 05:55_

---

_Merged by @dhruvmanila on 2024-06-12 05:59_

---

_Closed by @dhruvmanila on 2024-06-12 05:59_

---

_Branch deleted on 2024-06-12 05:59_

---

_Comment by @github-actions[bot] on 2024-06-12 06:15_

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
