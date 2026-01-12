```yaml
number: 8380
title: Avoid un-setting bracket flag in logical lines
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/set
created_at: 2023-10-31T14:16:17Z
updated_at: 2023-10-31T14:30:17Z
url: https://github.com/astral-sh/ruff/pull/8380
synced_at: 2026-01-10T23:40:55Z
```

# Avoid un-setting bracket flag in logical lines

---

_Pull request opened by @charliermarsh on 2023-10-31 14:16_

## Summary

By using `set`, we were setting the bracket flag to `false` if another operator was visited.

Closes https://github.com/astral-sh/ruff/issues/8379.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-10-31 14:16_

---

_Comment by @github-actions[bot] on 2023-10-31 14:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-10-31 14:30_

---

_Closed by @charliermarsh on 2023-10-31 14:30_

---

_Branch deleted on 2023-10-31 14:30_

---
