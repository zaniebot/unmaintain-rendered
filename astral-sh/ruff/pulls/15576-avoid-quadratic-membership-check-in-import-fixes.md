```yaml
number: 15576
title: Avoid quadratic membership check in import fixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/quad
created_at: 2025-01-18T22:56:28Z
updated_at: 2025-01-18T23:02:17Z
url: https://github.com/astral-sh/ruff/pull/15576
synced_at: 2026-01-12T15:55:51Z
```

# Avoid quadratic membership check in import fixes

---

_@charliermarsh_

## Summary

This leads to an explosion in runtime for (admittedly absurd) cases with tens of thousands of imports.


---

_Label `performance` added by @charliermarsh on 2025-01-18 22:56_

---

_Merged by @charliermarsh on 2025-01-18 23:01_

---

_Closed by @charliermarsh on 2025-01-18 23:01_

---

_Branch deleted on 2025-01-18 23:01_

---

_Comment by @github-actions[bot] on 2025-01-18 23:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
