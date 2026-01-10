```yaml
number: 9261
title: Remove special pre-visit for module docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/docstring
created_at: 2023-12-23T14:03:29Z
updated_at: 2023-12-23T15:03:13Z
url: https://github.com/astral-sh/ruff/pull/9261
synced_at: 2026-01-10T23:07:18Z
```

# Remove special pre-visit for module docstrings

---

_Pull request opened by @charliermarsh on 2023-12-23 14:03_

This ensures that we visit the module docstring like any other string.

Closes https://github.com/astral-sh/ruff/issues/9260.

---

_Label `bug` added by @charliermarsh on 2023-12-23 14:05_

---

_Comment by @github-actions[bot] on 2023-12-23 14:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-12-23 15:03_

---

_Closed by @charliermarsh on 2023-12-23 15:03_

---

_Branch deleted on 2023-12-23 15:03_

---
