```yaml
number: 14064
title: Detect items that hash to same value in duplicate sets
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-11-03T18:11:09Z
updated_at: 2024-11-03T18:58:21Z
url: https://github.com/astral-sh/ruff/pull/14064
synced_at: 2026-01-12T15:55:46Z
```

# Detect items that hash to same value in duplicate sets

---

_@charliermarsh_

## Summary

Like https://github.com/astral-sh/ruff/pull/14063, but ensures that we catch cases like `{1, True}` in which the items hash to the same value despite not being identical.


---

_Label `rule` added by @charliermarsh on 2024-11-03 18:11_

---

_Marked ready for review by @charliermarsh on 2024-11-03 18:11_

---

_Merged by @charliermarsh on 2024-11-03 18:49_

---

_Closed by @charliermarsh on 2024-11-03 18:49_

---

_Branch deleted on 2024-11-03 18:49_

---

_Comment by @github-actions[bot] on 2024-11-03 18:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
