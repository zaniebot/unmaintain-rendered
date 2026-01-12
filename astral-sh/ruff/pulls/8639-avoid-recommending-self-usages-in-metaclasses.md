```yaml
number: 8639
title: Avoid recommending Self usages in metaclasses
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/is-metaclass
created_at: 2023-11-12T22:55:01Z
updated_at: 2023-11-13T00:47:49Z
url: https://github.com/astral-sh/ruff/pull/8639
synced_at: 2026-01-10T23:40:55Z
```

# Avoid recommending Self usages in metaclasses

---

_Pull request opened by @charliermarsh on 2023-11-12 22:55_

PEP 673 forbids the use of `typing(_extensions).Self` in metaclasses, so we want to avoid flagging `PYI034` on metaclasses. This is based on an analogous change in `flake8-pyi`: https://github.com/PyCQA/flake8-pyi/pull/436.

Closes https://github.com/astral-sh/ruff/issues/8353.


---

_Label `bug` added by @charliermarsh on 2023-11-12 22:55_

---

_Comment by @github-actions[bot] on 2023-11-12 23:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-11-13 00:47_

---

_Closed by @charliermarsh on 2023-11-13 00:47_

---

_Branch deleted on 2023-11-13 00:47_

---
