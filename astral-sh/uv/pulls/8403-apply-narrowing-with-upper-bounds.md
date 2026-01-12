```yaml
number: 8403
title: Apply narrowing with upper bounds
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/requires-upper-bound
created_at: 2024-10-21T00:56:12Z
updated_at: 2024-10-21T01:03:51Z
url: https://github.com/astral-sh/uv/pull/8403
synced_at: 2026-01-12T16:08:18Z
```

# Apply narrowing with upper bounds

---

_@charliermarsh_

## Summary

If the user has an upper-bound in a `requires-python`, we don't correctly narrow it during resolution. We should be narrowing based on the intersection.

Closes #8297.


---

_Label `bug` added by @charliermarsh on 2024-10-21 01:02_

---

_Merged by @charliermarsh on 2024-10-21 01:03_

---

_Closed by @charliermarsh on 2024-10-21 01:03_

---

_Branch deleted on 2024-10-21 01:03_

---
