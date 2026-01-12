```yaml
number: 9408
title: Use transformed source code for diagnostic locations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/index
created_at: 2024-01-06T15:14:39Z
updated_at: 2024-01-06T15:28:08Z
url: https://github.com/astral-sh/ruff/pull/9408
synced_at: 2026-01-12T15:55:28Z
```

# Use transformed source code for diagnostic locations

---

_@charliermarsh_

## Summary

After we apply fixes, the source code might be transformed. And yet, we're using the _unmodified_ source code to compute locations in some cases (e.g., for displaying parse errors, or Jupyter Notebook cells). This can lead to subtle errors in reporting, or even panics. This PR modifies the linter to use the _transformed_ source code for such computations.

Closes https://github.com/astral-sh/ruff/issues/9407.

---

_Label `bug` added by @charliermarsh on 2024-01-06 15:14_

---

_Marked ready for review by @charliermarsh on 2024-01-06 15:14_

---

_Merged by @charliermarsh on 2024-01-06 15:22_

---

_Closed by @charliermarsh on 2024-01-06 15:22_

---

_Branch deleted on 2024-01-06 15:22_

---

_Comment by @github-actions[bot] on 2024-01-06 15:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
