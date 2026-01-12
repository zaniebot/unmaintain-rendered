```yaml
number: 7216
title: If we have no environment markers, choose the smallest wheel during resolution
type: issue
state: open
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-09-09T13:39:18Z
updated_at: 2024-09-09T15:11:43Z
url: https://github.com/astral-sh/uv/issues/7216
synced_at: 2026-01-12T15:59:11Z
```

# If we have no environment markers, choose the smallest wheel during resolution

---

_@charliermarsh_

In that case, if we have to download it, we can do as little work as necessary. For example, the NumPy wheels vary in size from 6 MB to 20 MB.

---

_Label `performance` added by @charliermarsh on 2024-09-09 13:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 15:11_

---
