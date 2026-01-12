```yaml
number: 7577
title: "Remove `Int` wrapper type from parser"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/int
created_at: 2023-09-21T16:53:32Z
updated_at: 2023-09-21T17:08:06Z
url: https://github.com/astral-sh/ruff/pull/7577
synced_at: 2026-01-12T02:39:10Z
```

# Remove `Int` wrapper type from parser

---

_Pull request opened by @charliermarsh on 2023-09-21 16:53_

## Summary

This is only used for the `level` field in relative imports (e.g., `from ..foo import bar`). It seems unnecessary to use a wrapper here, so this PR changes to a `u32` directly.

---

_Label `internal` added by @charliermarsh on 2023-09-21 16:53_

---

_Merged by @charliermarsh on 2023-09-21 17:01_

---

_Closed by @charliermarsh on 2023-09-21 17:01_

---

_Branch deleted on 2023-09-21 17:01_

---

_Comment by @github-actions[bot] on 2023-09-21 17:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
