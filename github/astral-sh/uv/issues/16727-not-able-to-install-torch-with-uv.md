---
number: 16727
title: Not able to install torch with uv
type: issue
state: closed
author: bk1205
labels:
  - bug
assignees: []
created_at: 2025-11-13T17:58:03Z
updated_at: 2025-11-13T18:07:56Z
url: https://github.com/astral-sh/uv/issues/16727
synced_at: 2026-01-07T13:12:19-06:00
---

# Not able to install torch with uv

---

_Issue opened by @bk1205 on 2025-11-13 17:58_

### Summary

<img width="1343" height="121" alt="Image" src="https://github.com/user-attachments/assets/0d5f174d-d892-495c-95ce-e0bd3dd105bd" />

I tried what has been given in hint still same exception
What I tried:
```
[tool.uv]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'win32' and platform_machine == 'AMD64'"
]
```

### Platform

Windows 11 64 bit

### Version

0.9.9

### Python version

cpython-3.12.12-windows-x86-none

---

_Label `bug` added by @bk1205 on 2025-11-13 17:58_

---

_Closed by @bk1205 on 2025-11-13 18:07_

---
