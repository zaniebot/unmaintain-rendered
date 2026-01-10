---
number: 13505
title: uv python list is showing same python instance twice on windows
type: issue
state: open
author: Andrej730
labels:
  - bug
assignees: []
created_at: 2025-05-17T09:22:07Z
updated_at: 2025-05-30T13:45:55Z
url: https://github.com/astral-sh/uv/issues/13505
synced_at: 2026-01-10T01:25:34Z
---

# uv python list is showing same python instance twice on windows

---

_Issue opened by @Andrej730 on 2025-05-17 09:22_

### Summary

See example below:
```
> uv python list
cpython-3.14.0a6-windows-x86_64-none                 <download available>
cpython-3.14.0a6+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.3-windows-x86_64-none                   <download available>
cpython-3.13.3+freethreaded-windows-x86_64-none      <download available>
cpython-3.12.10-windows-x86_64-none                  <download available>
cpython-3.11.12-windows-x86_64-none                  <download available>
cpython-3.11.9-windows-x86_64-none                   C:\software\Python311\python.exe
cpython-3.11.9-windows-x86_64-none                   C:\Software\Python311\python.exe
cpython-3.10.17-windows-x86_64-none                  <download available>
cpython-3.10.11-windows-x86_64-none                  C:\Software\Python310\python.exe
cpython-3.9.22-windows-x86_64-none                   <download available>
cpython-3.9.13-windows-x86_64-none                   L:\Software\Python39\python.exe
cpython-3.8.20-windows-x86_64-none                   <download available>
pypy-3.11.11-windows-x86_64-none                     <download available>
pypy-3.10.16-windows-x86_64-none                     <download available>
pypy-3.9.19-windows-x86_64-none                      <download available>
pypy-3.8.16-windows-x86_64-none                      <download available>
graalpy-3.11.0-windows-x86_64-none                   <download available>
graalpy-3.10.0-windows-x86_64-none                   <download available>
```

On windows paths are case insensitive, so I guess they should be shown as one entry either way, but I'm not sure where lowercase "software" is coming from:

```python
import os
import shutil

paths = os.environ["PATH"].split(os.pathsep)
for i, path in enumerate(paths, 1):
    if "python" not in path.lower():
        continue
    print(f"{i:2}: {path}")

#  9: C:\Software\Python311\
# 22: C:\Software\Python311\Scripts\
# 23: C:\Software\Python311\libs
# 70: c:\Users\Andrej\.vscode-insiders\extensions\ms-python.debugpy-2025.9.2025051201-win32-x64\bundled\scripts\noConfigScripts
# C:\Software\Python311\python.exe

print(shutil.which("python.exe"))
```

### Platform

Windows 11

### Version

uv 0.7.5 (9d1a14e1f 2025-05-16)

### Python version

_No response_

---

_Label `bug` added by @Andrej730 on 2025-05-17 09:22_

---

_Comment by @zanieb on 2025-05-18 20:57_

This looks like a possible duplicate of https://github.com/astral-sh/uv/issues/9979 — I thought we fixed this though.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-05-30 13:45_

---
