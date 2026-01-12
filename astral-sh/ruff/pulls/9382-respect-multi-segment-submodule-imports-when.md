```yaml
number: 9382
title: Respect multi-segment submodule imports when resolving qualified names
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/fix
created_at: 2024-01-03T16:09:07Z
updated_at: 2024-01-03T16:25:09Z
url: https://github.com/astral-sh/ruff/pull/9382
synced_at: 2026-01-12T15:55:28Z
```

# Respect multi-segment submodule imports when resolving qualified names

---

_@charliermarsh_

Ensures that if the user has `import collections.abc`, then `get_or_import_symbol` returns `collections.abc.Iterator` (or similar) when requested.

---

_Marked ready for review by @charliermarsh on 2024-01-03 16:12_

---

_Comment by @charliermarsh on 2024-01-03 16:12_

\cc @AlexWaygood 

---

_Label `bug` added by @charliermarsh on 2024-01-03 16:13_

---

_Merged by @charliermarsh on 2024-01-03 16:24_

---

_Closed by @charliermarsh on 2024-01-03 16:24_

---

_Branch deleted on 2024-01-03 16:24_

---

_Comment by @github-actions[bot] on 2024-01-03 16:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
