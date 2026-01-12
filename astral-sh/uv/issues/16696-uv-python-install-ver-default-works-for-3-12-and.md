```yaml
number: 16696
title: "uv python install <ver> --default works for 3.12 and 3.14 but not 3.15"
type: issue
state: closed
author: johnyesberg
labels:
  - bug
assignees: []
created_at: 2025-11-12T01:39:43Z
updated_at: 2025-11-12T18:34:00Z
url: https://github.com/astral-sh/uv/issues/16696
synced_at: 2026-01-12T16:02:36Z
```

# uv python install <ver> --default works for 3.12 and 3.14 but not 3.15

---

_@johnyesberg_

### Summary

When I run `uv python install 3.12 --default`, it installs `python3.12.exe` and `python.exe` and `python3.exe`.
The same is true for 3.14
But for 3.15, it only installs `python3.15.exe`, not `python.exe` and `python3.exe`.
I couldn't see any warning.
I tried adding --verbose, and there was no hint that it even attempted to link `python3.exe` or `python.exe`.
I couldn't see any information in the docs about some versions being unsuitable for default use.

### Platform

Windows 11 x86_64

### Version

10.0.26100 Build 26100

### Python version

3.12, 3.14, 3.15

---

_Label `bug` added by @johnyesberg on 2025-11-12 01:39_

---

_Comment by @johnyesberg on 2025-11-12 04:17_

**`> uv python install 3.12 --default --preview-features python-install-default --verbose`**
```
DEBUG uv 0.9.8 (85c5d3228 2025-11-07)
DEBUG Acquired lock for `AppData\Roaming\uv\python`
DEBUG Found `cpython-3.12.12-windows-x86_64-none` for request `3.12 (cpython-3.12-windows-x86_64-none)`
DEBUG Using request timeout of 30s
DEBUG Inspecting existing executable at `C:\Users\johny\.local\bin\python3.12.exe`
DEBUG Executable at `C:\Users\johny\.local\bin\python3.12.exe` is already for `cpython-3.12.12-windows-x86_64-none`
DEBUG Inspecting existing executable at `C:\Users\johny\.local\bin\python3.exe`
DEBUG Replacing existing executable for `cpython-3.14.0-windows-x86_64-none` at `C:\Users\johny\.local\bin\python3.exe` with executable for `cpython-3.12.12-windows-x86_64-none` since `--default` was requested`
DEBUG Updated executable at `C:\Users\johny\.local\bin\python3.exe` to cpython-3.12.12-windows-x86_64-none
DEBUG Inspecting existing executable at `C:\Users\johny\.local\bin\python.exe`
DEBUG Replacing existing executable for `cpython-3.14.0-windows-x86_64-none` at `C:\Users\johny\.local\bin\python.exe` with executable for `cpython-3.12.12-windows-x86_64-none` since `--default` was requested`
DEBUG Updated executable at `C:\Users\johny\.local\bin\python.exe` to cpython-3.12.12-windows-x86_64-none
Installed Python 3.12.12 in 24ms
 + cpython-3.12.12-windows-x86_64-none (python.exe, python3.exe)
DEBUG Released lock at `C:\Users\johny\AppData\Roaming\uv\python\.lock`
>
```

**`> uv python install 3.15 --default --preview-features python-install-default --verbose`**
```
DEBUG uv 0.9.8 (85c5d3228 2025-11-07)
DEBUG Acquired lock for `AppData\Roaming\uv\python`
DEBUG No installation found for request `3.15 (cpython-3.15-windows-x86_64-none)`
DEBUG Found download `cpython-3.15.0a1-windows-x86_64-none` for request `3.15 (cpython-3.15-windows-x86_64-none)`
DEBUG Using request timeout of 30s
DEBUG Inspecting existing executable at `C:\Users\johny\.local\bin\python3.15.exe`
DEBUG Executable at `C:\Users\johny\.local\bin\python3.15.exe` is already for `cpython-3.15.0a1-windows-x86_64-none`
Installed Python 3.15.0a1 in 18ms
 + cpython-3.15.0a1-windows-x86_64-none
DEBUG Released lock at `C:\Users\johny\AppData\Roaming\uv\python\.lock`
>
```

---

_Comment by @nooscraft on 2025-11-12 14:15_

@zanieb let me take a look at this :)

---

_Closed by @zanieb on 2025-11-12 18:34_

---
