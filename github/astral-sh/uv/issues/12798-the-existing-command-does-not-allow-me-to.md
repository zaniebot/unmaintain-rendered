---
number: 12798
title: The existing command does not allow me to directly install the Python interpreter into the environment directory when creating a virtual environment.
type: issue
state: closed
author: tian969
labels:
  - bug
assignees: []
created_at: 2025-04-10T02:53:03Z
updated_at: 2025-04-10T03:52:57Z
url: https://github.com/astral-sh/uv/issues/12798
synced_at: 2026-01-07T13:12:18-06:00
---

# The existing command does not allow me to directly install the Python interpreter into the environment directory when creating a virtual environment.

---

_Issue opened by @tian969 on 2025-04-10 02:53_

### Summary

I created a virtual environment on the server, but when I access it via a mounted disk, Python-related issues arise. I want Python to be installed directly inside the environment during 'uv venv myvenv -p 3.12', rather than referencing the system Python or a shared Python directory, to avoid problems when moving the environment.

### Platform

Ubuntu 20.04

### Version

uv 0.6.14

### Python version

Python 3.12

---

_Label `bug` added by @tian969 on 2025-04-10 02:53_

---

_Comment by @zanieb on 2025-04-10 03:52_

This is a duplicate of #7865 

See my comment at https://github.com/astral-sh/uv/issues/12298#issuecomment-2734452973 for more explanation.

---

_Closed by @zanieb on 2025-04-10 03:52_

---
