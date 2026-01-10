```yaml
number: 11147
title: Display the AST even with syntax errors
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - playground
assignees: []
merged: true
base: main
head: dhruv/playground-ast
created_at: 2024-04-25T15:33:07Z
updated_at: 2024-04-25T16:25:24Z
url: https://github.com/astral-sh/ruff/pull/11147
synced_at: 2026-01-10T22:37:01Z
```

# Display the AST even with syntax errors

---

_Pull request opened by @dhruvmanila on 2024-04-25 15:33_

## Summary

This PR updates the playground to display the AST even if it contains a syntax error. This could be useful for development and also to give a quick preview of what error recovery looks like.

Note that not all recovery is correct but this allows us to iterate quickly on what can be improved.

## Test Plan

Build the playground locally and test it.

<img width="1688" alt="Screenshot 2024-04-25 at 21 02 22" src="https://github.com/astral-sh/ruff/assets/67177269/2b94934c-4f2c-4a9a-9693-3d8460ed9d0b">


---

_Label `internal` added by @dhruvmanila on 2024-04-25 15:33_

---

_Label `playground` added by @dhruvmanila on 2024-04-25 15:33_

---

_Comment by @github-actions[bot] on 2024-04-25 15:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-25 16:04_

---

_Merged by @dhruvmanila on 2024-04-25 16:25_

---

_Closed by @dhruvmanila on 2024-04-25 16:25_

---

_Branch deleted on 2024-04-25 16:25_

---
