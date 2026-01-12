```yaml
number: 12537
title: Avoid marking required imports as unused
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/required-import-remove
created_at: 2024-07-26T17:15:43Z
updated_at: 2024-07-26T18:23:44Z
url: https://github.com/astral-sh/ruff/pull/12537
synced_at: 2026-01-12T15:55:41Z
```

# Avoid marking required imports as unused

---

_@charliermarsh_

## Summary

If an import is marked as "required", we should never flag it as unused. In practice, this is rare, since required imports are typically used for `__future__` annotations, which are always considered "used".

Closes https://github.com/astral-sh/ruff/issues/12458.


---

_Label `bug` added by @charliermarsh on 2024-07-26 17:15_

---

_Comment by @github-actions[bot] on 2024-07-26 17:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-07-26 18:23_

---

_Closed by @charliermarsh on 2024-07-26 18:23_

---

_Branch deleted on 2024-07-26 18:23_

---
