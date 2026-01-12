```yaml
number: 9277
title: Avoid adding return types to stub methods
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/abstract
created_at: 2023-12-25T13:47:10Z
updated_at: 2023-12-25T14:06:10Z
url: https://github.com/astral-sh/ruff/pull/9277
synced_at: 2026-01-12T15:55:28Z
```

# Avoid adding return types to stub methods

---

_@charliermarsh_

We should avoid adding `-> None` to stubs in `.pyi` files, along with a few other cases. (We already ignore abstract methods.)

Closes https://github.com/astral-sh/ruff/issues/9270.

---

_Label `bug` added by @charliermarsh on 2023-12-25 13:47_

---

_Merged by @charliermarsh on 2023-12-25 14:03_

---

_Closed by @charliermarsh on 2023-12-25 14:03_

---

_Branch deleted on 2023-12-25 14:03_

---

_Comment by @github-actions[bot] on 2023-12-25 14:06_

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
