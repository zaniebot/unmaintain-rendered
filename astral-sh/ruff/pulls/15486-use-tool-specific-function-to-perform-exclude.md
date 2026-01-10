```yaml
number: 15486
title: Use tool specific function to perform exclude checks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/tool-specific-exclude-fn
created_at: 2025-01-15T06:29:22Z
updated_at: 2025-01-15T07:48:47Z
url: https://github.com/astral-sh/ruff/pull/15486
synced_at: 2026-01-10T20:34:00Z
```

# Use tool specific function to perform exclude checks

---

_Pull request opened by @dhruvmanila on 2025-01-15 06:29_

## Summary

This PR creates separate functions to check whether the document path is excluded for linting or formatting. The main motivation is to avoid the double `Option` for the call sites and makes passing the correct settings simpler.


---

_Label `internal` added by @dhruvmanila on 2025-01-15 06:29_

---

_Comment by @github-actions[bot] on 2025-01-15 06:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2025-01-15 07:48_

---

_Closed by @dhruvmanila on 2025-01-15 07:48_

---

_Branch deleted on 2025-01-15 07:48_

---
