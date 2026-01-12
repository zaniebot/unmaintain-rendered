```yaml
number: 11523
title: "Treat all `singledispatch` arguments as runtime-required"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/singledispatch
created_at: 2024-05-24T00:22:14Z
updated_at: 2024-05-24T00:36:25Z
url: https://github.com/astral-sh/ruff/pull/11523
synced_at: 2026-01-12T15:55:38Z
```

# Treat all `singledispatch` arguments as runtime-required

---

_@charliermarsh_

## Summary

It turns out that `singledispatch` does end up evaluating all arguments, even though only the first is used to dispatch.

Closes https://github.com/astral-sh/ruff/issues/11520.


---

_Label `bug` added by @charliermarsh on 2024-05-24 00:22_

---

_Comment by @github-actions[bot] on 2024-05-24 00:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-05-24 00:36_

---

_Closed by @charliermarsh on 2024-05-24 00:36_

---

_Branch deleted on 2024-05-24 00:36_

---
