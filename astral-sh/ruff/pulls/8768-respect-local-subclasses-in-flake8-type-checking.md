```yaml
number: 8768
title: "Respect local subclasses in `flake8-type-checking`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pyi
created_at: 2023-11-19T14:39:04Z
updated_at: 2023-11-19T14:52:28Z
url: https://github.com/astral-sh/ruff/pull/8768
synced_at: 2026-01-10T23:40:55Z
```

# Respect local subclasses in `flake8-type-checking`

---

_Pull request opened by @charliermarsh on 2023-11-19 14:39_

If you define a subclass of `pydantic.BaseModel`, and then a subclass of _that_ class in the same file, we'll now correctly treat it as runtime-evaluated.

Closes https://github.com/astral-sh/ruff/issues/7893.

---

_Label `bug` added by @charliermarsh on 2023-11-19 14:39_

---

_Merged by @charliermarsh on 2023-11-19 14:49_

---

_Closed by @charliermarsh on 2023-11-19 14:49_

---

_Branch deleted on 2023-11-19 14:49_

---

_Comment by @github-actions[bot] on 2023-11-19 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
