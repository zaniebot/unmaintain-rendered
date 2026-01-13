```yaml
number: 17430
title: Access is Denied on Windows with uv sync. OS Code Error -2147024891
type: issue
state: open
author: peroksid
labels:
  - bug
assignees: []
created_at: 2026-01-12T23:30:15Z
updated_at: 2026-01-13T11:23:07Z
url: https://github.com/astral-sh/uv/issues/17430
synced_at: 2026-01-13T12:25:18Z
```

# Access is Denied on Windows with uv sync. OS Code Error -2147024891

---

_@peroksid_

### Summary

In the corporate environment, in Windows 11, with some security programs running, on a local drive, in a virtual machine, `uv sync` throws occasional "Access is denied", and in the end of installing dependencies consistently throws "Access is denied" for the editable or non-editable package itself.

It happens in Python 3.11, 3.12, 3.13 for uv >0.9.8, <= 0.9.20.

We pinned to 0.9.8, and it stopped happening.

### Platform

Windows 11 x86_64

### Version

uv >0.9.8, <= 0.9.20

### Python version

Python 3.11, 3.12, 3.13

---

_Label `bug` added by @peroksid on 2026-01-12 23:30_

---

_Comment by @zanieb on 2026-01-12 23:33_

Can you reproduce on the latest version too?

---

_Comment by @peroksid on 2026-01-13 11:23_

Only up to 0.9.21. Can't try the newer versions because they are not available in the pypi proxy mirror.

---
