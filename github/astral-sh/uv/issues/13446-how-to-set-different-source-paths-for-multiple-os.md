---
number: 13446
title: How to set different source paths for multiple os
type: issue
state: closed
author: triangle959
labels:
  - question
assignees: []
created_at: 2025-05-14T08:20:07Z
updated_at: 2025-05-15T02:38:01Z
url: https://github.com/astral-sh/uv/issues/13446
synced_at: 2026-01-07T13:12:18-06:00
---

# How to set different source paths for multiple os

---

_Issue opened by @triangle959 on 2025-05-14 08:20_

### Question

If I were developing locally, there would be no problem using uv.lock below. 
```
[[package]]
name = "gevent"
version = "23.9.0"
source = { path = "dependencies/gevent-23.9.0-cp310-cp310-macosx_11_0_x86_64.whl" }
dependencies = [
    { name = "cffi", marker = "platform_python_implementation == 'CPython' and sys_platform == 'win32'" },
    { name = "greenlet", marker = "platform_python_implementation == 'CPython'" },
    { name = "zope-event" },
    { name = "zope-interface" },
]
wheels = [
    { filename = "gevent-23.9.0-cp310-cp310-macosx_11_0_x86_64.whl", hash = "sha256:1b9ff212cddf245d44651600970c5370c4168f710d4856d8b2350d734dfff1a6" },
]
```
But if I deploy the service on a non macOS system, how do I set different source paths?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @triangle959 on 2025-05-14 08:20_

---

_Comment by @konstin on 2025-05-14 08:23_

One option is to use a flat index, a directory with wheel files, instead of using a path to a single file (https://docs.astral.sh/uv/configuration/indexes/#flat-indexes).

---

_Comment by @charliermarsh on 2025-05-15 02:37_

Typically, you'd do this:

```toml
[tool.uv.sources]
gevent = [
    { path = "dependencies/gevent-23.9.0-cp310-cp310-macosx_11_0_x86_64.whl", marker = "sys_platform == 'darwin'" },
    { path = "dependencies/gevent-23.9.0-cp310-cp310-linux_x86_64.whl", marker = "sys_platform == 'linux'" },
]
```

See: https://docs.astral.sh/uv/concepts/projects/dependencies/#platform-specific-sources

---

_Closed by @charliermarsh on 2025-05-15 02:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-15 02:38_

---
