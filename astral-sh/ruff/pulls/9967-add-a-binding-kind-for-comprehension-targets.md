```yaml
number: 9967
title: Add a binding kind for comprehension targets
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comp-var
created_at: 2024-02-13T00:38:28Z
updated_at: 2024-02-13T01:09:40Z
url: https://github.com/astral-sh/ruff/pull/9967
synced_at: 2026-01-12T15:55:30Z
```

# Add a binding kind for comprehension targets

---

_@charliermarsh_

## Summary

I was surprised to learn that we treat `x` in `[_ for x in y]` as an "assignment" binding kind, rather than a dedicated comprehension variable.

---

_Comment by @github-actions[bot] on 2024-02-13 00:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @charliermarsh on 2024-02-13 01:09_

---

_Label `internal` added by @charliermarsh on 2024-02-13 01:09_

---

_Merged by @charliermarsh on 2024-02-13 01:09_

---

_Closed by @charliermarsh on 2024-02-13 01:09_

---

_Branch deleted on 2024-02-13 01:09_

---
