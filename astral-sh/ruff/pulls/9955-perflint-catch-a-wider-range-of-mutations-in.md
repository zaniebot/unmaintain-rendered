```yaml
number: 9955
title: "[`perflint`] Catch a wider range of mutations in `PERF101`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/append
created_at: 2024-02-12T16:59:45Z
updated_at: 2024-02-12T17:18:02Z
url: https://github.com/astral-sh/ruff/pull/9955
synced_at: 2026-01-10T22:57:09Z
```

# [`perflint`] Catch a wider range of mutations in `PERF101`

---

_Pull request opened by @charliermarsh on 2024-02-12 16:59_

## Summary

This PR ensures that if a list `x` is modified within a `for` loop, we avoid flagging `list(x)` as unnecessary. Previously, we only detected calls to exactly `.append`, and they couldn't be nested within other statements.

Closes https://github.com/astral-sh/ruff/issues/9925.


---

_Label `bug` added by @charliermarsh on 2024-02-12 17:00_

---

_Marked ready for review by @charliermarsh on 2024-02-12 17:00_

---

_Merged by @charliermarsh on 2024-02-12 17:17_

---

_Closed by @charliermarsh on 2024-02-12 17:17_

---

_Branch deleted on 2024-02-12 17:17_

---

_Comment by @github-actions[bot] on 2024-02-12 17:18_

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
