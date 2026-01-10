```yaml
number: 9332
title: Fix continuation detection following multi-line strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/fuzz
created_at: 2023-12-31T15:35:13Z
updated_at: 2023-12-31T15:51:09Z
url: https://github.com/astral-sh/ruff/pull/9332
synced_at: 2026-01-10T23:07:18Z
```

# Fix continuation detection following multi-line strings

---

_Pull request opened by @charliermarsh on 2023-12-31 15:35_

## Summary

The logic that detects continuations assumed that tokens themselves cannot span multiple lines. However, strings _can_ -- even single-quoted strings.

Closes https://github.com/astral-sh/ruff/issues/9323.


---

_Label `bug` added by @charliermarsh on 2023-12-31 15:35_

---

_Label `fuzzer` added by @charliermarsh on 2023-12-31 15:35_

---

_Marked ready for review by @charliermarsh on 2023-12-31 15:35_

---

_Merged by @charliermarsh on 2023-12-31 15:43_

---

_Closed by @charliermarsh on 2023-12-31 15:43_

---

_Branch deleted on 2023-12-31 15:43_

---

_Comment by @github-actions[bot] on 2023-12-31 15:51_

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
