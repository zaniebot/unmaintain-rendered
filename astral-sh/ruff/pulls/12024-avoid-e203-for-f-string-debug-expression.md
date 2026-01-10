```yaml
number: 12024
title: "Avoid `E203` for f-string debug expression"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/debug-expression
created_at: 2024-06-25T08:48:35Z
updated_at: 2024-06-25T09:30:32Z
url: https://github.com/astral-sh/ruff/pull/12024
synced_at: 2026-01-10T21:56:00Z
```

# Avoid `E203` for f-string debug expression

---

_Pull request opened by @dhruvmanila on 2024-06-25 08:48_

## Summary

This PR fixes a bug where Ruff would raise `E203` for f-string debug expression. This isn't valid because whitespaces are important for debug expressions.

fixes: #12023

## Test Plan

Add test case and make sure there are no snapshot changes.

---

_Label `bug` added by @dhruvmanila on 2024-06-25 08:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-25 08:52_

---

_Comment by @github-actions[bot] on 2024-06-25 09:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-06-25 09:14_

---

_Merged by @dhruvmanila on 2024-06-25 09:30_

---

_Closed by @dhruvmanila on 2024-06-25 09:30_

---

_Branch deleted on 2024-06-25 09:30_

---
