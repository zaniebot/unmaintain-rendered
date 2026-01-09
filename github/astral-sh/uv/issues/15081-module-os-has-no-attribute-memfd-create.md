---
number: 15081
title: "Module 'os' has no attribute 'memfd_create'"
type: issue
state: closed
author: p-gentili
labels:
  - bug
assignees: []
created_at: 2025-08-05T13:24:43Z
updated_at: 2025-08-05T13:36:36Z
url: https://github.com/astral-sh/uv/issues/15081
synced_at: 2026-01-07T13:12:19-06:00
---

# Module 'os' has no attribute 'memfd_create'

---

_Issue opened by @p-gentili on 2025-08-05 13:24_

### Summary

[os.memfd_create](https://docs.python.org/3/library/os.html#os.memfd_create) is a method only supported in "Linux >= 3.17 with glibc >= 2.27" but it's missing from uv python builds (at least 3.10, 3.12).

### Step to reproduce
```
uv python install 3.12
uv run python -c "import os; os.memfd_create"
# AttributeError: module 'os' has no attribute 'memfd_create'
```

Prove the system supports it
```
uv run python -c "import ctypes; print(hasattr(ctypes.CDLL('libc.so.6'), 'memfd_create'))"
# True
```

### Platform

Linux 6.14.0-27-generic x86_64 GNU/Linux

### Version

uv 0.8.4

### Python version

Python 3.12.11

---

_Label `bug` added by @p-gentili on 2025-08-05 13:24_

---

_Comment by @charliermarsh on 2025-08-05 13:33_

Thanks! I think this is the same as https://github.com/astral-sh/python-build-standalone/issues/193 --  does that sound right? We should probably track it there since it's a Python build problem.

---

_Comment by @p-gentili on 2025-08-05 13:35_

Oh yes indeed, couldn't find the issue. Thanks for pointing it out.

---

_Closed by @p-gentili on 2025-08-05 13:35_

---

_Comment by @charliermarsh on 2025-08-05 13:36_

No prob.

---
