```yaml
number: 4648
title: Merge markers when applying constraints
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/constraint-markers
created_at: 2024-06-29T16:39:53Z
updated_at: 2024-06-29T16:51:05Z
url: https://github.com/astral-sh/uv/pull/4648
synced_at: 2026-01-12T16:06:22Z
```

# Merge markers when applying constraints

---

_@charliermarsh_

## Summary

When a constraint is applied to a requirement with a marker, the marker needs to be propagated to the constraint.

If both the constraint and the requirement have a marker, they need to be merged together (via `and`).

Closes https://github.com/astral-sh/uv/issues/4575.


---

_Label `bug` added by @charliermarsh on 2024-06-29 16:39_

---

_Merged by @charliermarsh on 2024-06-29 16:51_

---

_Closed by @charliermarsh on 2024-06-29 16:51_

---

_Branch deleted on 2024-06-29 16:51_

---
