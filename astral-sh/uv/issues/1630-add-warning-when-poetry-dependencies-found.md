```yaml
number: 1630
title: "Add warning when `poetry` dependencies found instead of PEP 621 dependencies"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2024-02-18T07:47:44Z
updated_at: 2024-02-20T00:08:27Z
url: https://github.com/astral-sh/uv/issues/1630
synced_at: 2026-01-12T15:58:30Z
```

# Add warning when `poetry` dependencies found instead of PEP 621 dependencies

---

_@zanieb_

This results in an empty lock file and has caused some confusion:

- https://github.com/astral-sh/uv/issues/1619
- https://github.com/astral-sh/uv/issues/1597

We can detect this case and warn.

---

_Label `error messages` added by @zanieb on 2024-02-18 07:47_

---

_Label `good first issue` added by @zanieb on 2024-02-18 07:47_

---

_Closed by @charliermarsh on 2024-02-20 00:08_

---
