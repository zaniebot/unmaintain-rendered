```yaml
number: 9214
title: "Allow removal of `typing` from `exempt-modules`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/move
created_at: 2023-12-20T15:49:09Z
updated_at: 2023-12-20T16:03:03Z
url: https://github.com/astral-sh/ruff/pull/9214
synced_at: 2026-01-12T15:55:28Z
```

# Allow removal of `typing` from `exempt-modules`

---

_@charliermarsh_

## Summary

If you remove `typing` from `exempt-modules`, we tend to panic, since we try to add `TYPE_CHECKING` to `from typing import ...` statements while concurrently attempting to remove other members from that import. This PR adds special-casing for typing imports to avoid such panics.

Closes https://github.com/astral-sh/ruff/issues/5331
Closes https://github.com/astral-sh/ruff/issues/9196.
Closes https://github.com/astral-sh/ruff/issues/9197.

---

_Label `bug` added by @charliermarsh on 2023-12-20 15:49_

---

_Comment by @github-actions[bot] on 2023-12-20 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-12-20 16:03_

---

_Closed by @charliermarsh on 2023-12-20 16:03_

---

_Branch deleted on 2023-12-20 16:03_

---
