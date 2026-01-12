```yaml
number: 7968
title: Treat type aliases as typing-only expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tch-pep
created_at: 2023-10-16T00:00:43Z
updated_at: 2023-10-16T00:17:10Z
url: https://github.com/astral-sh/ruff/pull/7968
synced_at: 2026-01-12T02:32:41Z
```

# Treat type aliases as typing-only expressions

---

_Pull request opened by @charliermarsh on 2023-10-16 00:00_

## Summary

Given `type RecordOrThings = Record | int | str`, the right-hand side won't be evaluated at runtime. Same goes for `Record` in `type RecordCallback[R: Record] = Callable[[R], None]`. This PR modifies the visitation logic to treat them as typing-only.

Closes https://github.com/astral-sh/ruff/issues/7966.


---

_Label `bug` added by @charliermarsh on 2023-10-16 00:00_

---

_Merged by @charliermarsh on 2023-10-16 00:09_

---

_Closed by @charliermarsh on 2023-10-16 00:09_

---

_Branch deleted on 2023-10-16 00:09_

---

_Comment by @github-actions[bot] on 2023-10-16 00:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
