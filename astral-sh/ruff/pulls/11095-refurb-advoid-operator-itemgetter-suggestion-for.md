```yaml
number: 11095
title: "[`refurb`] Advoid `operator.itemgetter` suggestion for single-item tuple"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/furb
created_at: 2024-04-23T00:12:22Z
updated_at: 2024-04-23T00:26:27Z
url: https://github.com/astral-sh/ruff/pull/11095
synced_at: 2026-01-12T15:55:34Z
```

# [`refurb`] Advoid `operator.itemgetter` suggestion for single-item tuple

---

_@charliermarsh_

## Summary

The `operator.itemgetter` behavior changes where there's more than one argument, such that `operator.itemgetter(0)` yields `r[0]`, rather than `(r[0],)`.

Closes https://github.com/astral-sh/ruff/issues/11075.


---

_Label `bug` added by @charliermarsh on 2024-04-23 00:12_

---

_Merged by @charliermarsh on 2024-04-23 00:20_

---

_Closed by @charliermarsh on 2024-04-23 00:20_

---

_Branch deleted on 2024-04-23 00:20_

---

_Comment by @github-actions[bot] on 2024-04-23 00:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
