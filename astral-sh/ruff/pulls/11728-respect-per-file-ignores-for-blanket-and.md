```yaml
number: 11728
title: Respect per-file ignores for blanket and redirected noqa rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/noqa
created_at: 2024-06-04T03:54:25Z
updated_at: 2024-06-04T04:08:44Z
url: https://github.com/astral-sh/ruff/pull/11728
synced_at: 2026-01-10T21:56:00Z
```

# Respect per-file ignores for blanket and redirected noqa rules

---

_Pull request opened by @charliermarsh on 2024-06-04 03:54_

## Summary

Ensures that we respect per-file ignores and exemptions for these rules. Specifically, we allow:

```python
# ruff: noqa: PGH004
```

...to ignore `PGH004`.


---

_Label `bug` added by @charliermarsh on 2024-06-04 03:54_

---

_Merged by @charliermarsh on 2024-06-04 03:58_

---

_Closed by @charliermarsh on 2024-06-04 03:58_

---

_Branch deleted on 2024-06-04 03:58_

---

_Comment by @github-actions[bot] on 2024-06-04 04:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
