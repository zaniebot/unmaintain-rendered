```yaml
number: 8785
title: "[`pep8-naming`] Avoid `N806` errors for type alias statements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/naming
created_at: 2023-11-20T12:24:06Z
updated_at: 2023-11-20T12:36:30Z
url: https://github.com/astral-sh/ruff/pull/8785
synced_at: 2026-01-10T23:40:55Z
```

# [`pep8-naming`] Avoid `N806` errors for type alias statements

---

_Pull request opened by @charliermarsh on 2023-11-20 12:24_

Allow, e.g.:

```python
def func():
    type MyInt = int
```

(We already allowed `MyInt: TypeAlias = int`.)

Closes https://github.com/astral-sh/ruff/issues/8773.

---

_Label `bug` added by @charliermarsh on 2023-11-20 12:24_

---

_Merged by @charliermarsh on 2023-11-20 12:28_

---

_Closed by @charliermarsh on 2023-11-20 12:28_

---

_Branch deleted on 2023-11-20 12:28_

---

_Comment by @github-actions[bot] on 2023-11-20 12:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
