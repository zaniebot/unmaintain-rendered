```yaml
number: 14058
title: "[`flake8-bugbear`] - do not run `mutable-argument-default` on stubs (`B006`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-B006-stubs
created_at: 2024-11-02T22:09:56Z
updated_at: 2024-11-03T02:48:51Z
url: https://github.com/astral-sh/ruff/pull/14058
synced_at: 2026-01-10T20:59:37Z
```

# [`flake8-bugbear`] - do not run `mutable-argument-default` on stubs (`B006`)

---

_Pull request opened by @diceroll123 on 2024-11-02 22:09_

## Summary

Early-exits in `B006` when the file is a stub. Fixes #14026 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-11-02 22:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-11-03 02:48_

---

_Closed by @charliermarsh on 2024-11-03 02:48_

---

_Label `bug` added by @charliermarsh on 2024-11-03 02:48_

---
