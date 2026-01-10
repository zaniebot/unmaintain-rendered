```yaml
number: 3683
title: "Support `in` operators with `python_version` marker"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-05-21T00:24:40Z
updated_at: 2024-08-19T14:10:22Z
url: https://github.com/astral-sh/uv/issues/3683
synced_at: 2026-01-10T04:53:49Z
```

# Support `in` operators with `python_version` marker

---

_Issue opened by @charliermarsh on 2024-05-21 00:24_

E.g., `python_version in "2.6 2.7 3.2 3.3"` should be evaluate as `python_version == "2.6" or ...`.

See: https://github.com/astral-sh/uv/issues/3675.


---

_Label `bug` added by @charliermarsh on 2024-05-21 00:24_

---

_Comment by @charliermarsh on 2024-05-21 00:24_

Right now, these are evaluate as `Arbitrary`.

---

_Assigned to @zanieb by @zanieb on 2024-08-17 16:32_

---

_Comment by @zanieb on 2024-08-17 17:01_

Working on this... 

---

_Closed by @zanieb on 2024-08-19 14:10_

---
