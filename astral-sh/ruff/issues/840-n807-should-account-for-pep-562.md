```yaml
number: 840
title: N807 should account for PEP 562
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2022-11-20T22:35:44Z
updated_at: 2022-11-20T23:43:21Z
url: https://github.com/astral-sh/ruff/issues/840
synced_at: 2026-01-10T12:09:58Z
```

# N807 should account for PEP 562

---

_Issue opened by @ofek on 2022-11-20 22:35_

`__getattr__` / `__dir__` - https://peps.python.org/pep-0562/

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-20 22:50_

---

_Comment by @charliermarsh on 2022-11-20 22:50_

I can fix this.

---

_Closed by @charliermarsh on 2022-11-20 22:55_

---

_Comment by @ofek on 2022-11-20 23:27_

Thanks!

---

_Comment by @charliermarsh on 2022-11-20 23:38_

Should be live now in [v0.0.132](https://github.com/charliermarsh/ruff/releases/tag/v0.0.132), but let me know if you see otherwise.

---

_Comment by @ofek on 2022-11-20 23:43_

Success https://github.com/pypa/hatch/pull/609

---
