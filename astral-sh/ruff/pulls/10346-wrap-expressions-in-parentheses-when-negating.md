```yaml
number: 10346
title: Wrap expressions in parentheses when negating
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2024-03-11T21:52:52Z
updated_at: 2024-03-11T22:20:56Z
url: https://github.com/astral-sh/ruff/pull/10346
synced_at: 2026-01-10T22:47:01Z
```

# Wrap expressions in parentheses when negating

---

_Pull request opened by @charliermarsh on 2024-03-11 21:52_

## Summary

When negating an expression like `a or b`, we need to wrap it in parentheses, e.g., `not (a or b)` instead of `not a or b`, due to operator precedence.

Closes https://github.com/astral-sh/ruff/issues/10335.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-03-11 21:52_

---

_Comment by @github-actions[bot] on 2024-03-11 22:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-11 22:20_

---

_Closed by @charliermarsh on 2024-03-11 22:20_

---

_Branch deleted on 2024-03-11 22:20_

---
