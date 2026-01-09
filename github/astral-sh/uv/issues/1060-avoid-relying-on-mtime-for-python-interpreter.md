---
number: 1060
title: "Avoid relying on `mtime` for Python interpreter caching"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-23T14:21:57Z
updated_at: 2024-01-24T00:16:26Z
url: https://github.com/astral-sh/uv/issues/1060
synced_at: 2026-01-07T13:12:16-06:00
---

# Avoid relying on `mtime` for Python interpreter caching

---

_Issue opened by @charliermarsh on 2024-01-23 14:21_

At the very least, we should use `ctime`, which changes whenever `mtime` changes (and more).

See: https://apenwarr.ca/log/20181113.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-23 14:21_

---

_Label `bug` added by @charliermarsh on 2024-01-23 14:22_

---

_Comment by @charliermarsh on 2024-01-24 00:16_

Closed by #1067.

---

_Closed by @charliermarsh on 2024-01-24 00:16_

---

_Referenced in [astral-sh/uv#9263](../../astral-sh/uv/issues/9263.md) on 2024-11-20 03:52_

---
