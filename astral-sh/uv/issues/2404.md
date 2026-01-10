```yaml
number: 2404
title: Create site packages directory if missing
type: issue
state: closed
author: zanieb
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-03-13T03:59:30Z
updated_at: 2024-03-13T15:10:35Z
url: https://github.com/astral-sh/uv/issues/2404
synced_at: 2026-01-10T05:40:32Z
```

# Create site packages directory if missing

---

_Issue opened by @zanieb on 2024-03-13 03:59_

We fail if the directory does not exist when installing but pip does not (it creates it).

Needed for:

- #2403
- #2402 

---

_Label `bug` added by @zanieb on 2024-03-13 03:59_

---

_Label `compatibility` added by @zanieb on 2024-03-13 03:59_

---

_Comment by @charliermarsh on 2024-03-13 04:01_

I suspect this is also https://github.com/astral-sh/uv/issues/2353.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-13 14:24_

---

_Closed by @charliermarsh on 2024-03-13 15:10_

---
