```yaml
number: 15575
title: Apply redefinition fixes by source code order
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/o
created_at: 2025-01-18T22:37:35Z
updated_at: 2025-01-18T22:46:12Z
url: https://github.com/astral-sh/ruff/pull/15575
synced_at: 2026-01-10T20:05:43Z
```

# Apply redefinition fixes by source code order

---

_Pull request opened by @charliermarsh on 2025-01-18 22:37_

## Summary

Right now, these are being applied in random order, since if we have two `RedefinitionWhileUnused`, it just takes the first-generated (whereas the next comparator in the sort here orders by location)... Which means we frequently have to re-run!


---

_Label `bug` added by @charliermarsh on 2025-01-18 22:38_

---

_Label `fixes` added by @charliermarsh on 2025-01-18 22:38_

---

_Merged by @charliermarsh on 2025-01-18 22:44_

---

_Closed by @charliermarsh on 2025-01-18 22:44_

---

_Branch deleted on 2025-01-18 22:44_

---

_Comment by @github-actions[bot] on 2025-01-18 22:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
