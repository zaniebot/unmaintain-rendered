```yaml
number: 8881
title: Avoid filtering out un-representable types in return annotation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/an
created_at: 2023-11-28T21:03:51Z
updated_at: 2023-11-28T21:17:45Z
url: https://github.com/astral-sh/ruff/pull/8881
synced_at: 2026-01-12T15:55:27Z
```

# Avoid filtering out un-representable types in return annotation

---

_@charliermarsh_

## Summary

Given `Union[Dict, None]` (in our internal representation), we were filtering out `Dict` since we treat it as un-representable (i.e., we can't convert it to an expression), returning just `None` as the type annotation. We should require that all members of the union are representable.

Closes https://github.com/astral-sh/ruff/issues/8879.


---

_Label `bug` added by @charliermarsh on 2023-11-28 21:05_

---

_Merged by @charliermarsh on 2023-11-28 21:10_

---

_Closed by @charliermarsh on 2023-11-28 21:10_

---

_Branch deleted on 2023-11-28 21:10_

---

_Comment by @github-actions[bot] on 2023-11-28 21:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
