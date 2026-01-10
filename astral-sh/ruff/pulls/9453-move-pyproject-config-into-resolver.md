```yaml
number: 9453
title: "Move `pyproject_config` into `Resolver`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/resolver
created_at: 2024-01-10T03:21:34Z
updated_at: 2024-01-10T22:58:54Z
url: https://github.com/astral-sh/ruff/pull/9453
synced_at: 2026-01-10T22:57:09Z
```

# Move `pyproject_config` into `Resolver`

---

_Pull request opened by @charliermarsh on 2024-01-10 03:21_

## Summary

Sort of a random PR to make the coupling between `pyproject_config` and `resolver` more explicit by passing it to the `Resolver`, rather than threading it through to each individual method.

---

_Label `internal` added by @charliermarsh on 2024-01-10 03:22_

---

_Comment by @github-actions[bot] on 2024-01-10 03:43_

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

_@zanieb approved on 2024-01-10 05:00_

---

_Marked ready for review by @charliermarsh on 2024-01-10 21:43_

---

_Merged by @charliermarsh on 2024-01-10 22:58_

---

_Closed by @charliermarsh on 2024-01-10 22:58_

---

_Branch deleted on 2024-01-10 22:58_

---
