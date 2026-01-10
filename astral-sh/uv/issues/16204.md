```yaml
number: 16204
title: "`uv run python --with numpy python -V` fails because Scripts directory doesn't exist"
type: issue
state: closed
author: pfmoore
labels:
  - bug
assignees: []
created_at: 2025-10-09T11:32:46Z
updated_at: 2025-10-09T15:17:16Z
url: https://github.com/astral-sh/uv/issues/16204
synced_at: 2026-01-10T03:23:54Z
```

# `uv run python --with numpy python -V` fails because Scripts directory doesn't exist

---

_Issue opened by @pfmoore on 2025-10-09 11:32_

### Summary

I have just upgraded my Python installation (on Windows) to Python 3.14, using the new Python Manager for Windows. I uninstalled all older versions of Python, and installed 3.14 and 3.14t. I also did `uv python list`, and uninstalled all uv-managed Pythons, so the only Python installations uv sees are the two 3.14 ones:

```
❯ uv python list
cpython-3.14.0-windows-x86_64-none                 C:\Users\Gustav\AppData\Local\Python\pythoncore-3.14-64\python.exe
cpython-3.14.0-windows-x86_64-none                 <download available>
cpython-3.14.0+freethreaded-windows-x86_64-none    C:\Users\Gustav\AppData\Local\Python\pythoncore-3.14t-64\python3.14t.exe
cpython-3.14.0+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.8-windows-x86_64-none                 <download available>
cpython-3.13.8+freethreaded-windows-x86_64-none    <download available>
cpython-3.12.11-windows-x86_64-none                <download available>
cpython-3.11.13-windows-x86_64-none                <download available>
cpython-3.10.18-windows-x86_64-none                <download available>
cpython-3.9.23-windows-x86_64-none                 <download available>
cpython-3.8.20-windows-x86_64-none                 <download available>
pypy-3.11.13-windows-x86_64-none                   <download available>
pypy-3.10.16-windows-x86_64-none                   <download available>
pypy-3.9.19-windows-x86_64-none                    <download available>
pypy-3.8.16-windows-x86_64-none                    <download available>
graalpy-3.12.0-windows-x86_64-none                 <download available>
graalpy-3.11.0-windows-x86_64-none                 <download available>
graalpy-3.10.0-windows-x86_64-none                 <download available>
```

When I run `uv run python -V`, I get the output `Python 3.14.0`, as expected. However, when I run `uv run --with numpy python -V`, I get

```
❯ uv run --with numpy python -V
error: failed to read directory `C:\Users\Gustav\AppData\Local\Python\pythoncore-3.14-64\Scripts`: The system cannot find the path specified. (os error 3)
```

This is technically correct, as I do not have a `Scripts` directory (yet?) in that location. But the `Scripts` directory is not needed to run Python, so it should not be necessary to manually modify my Python installation just to get `uv run --with` to work.

### Platform

Windows 11 x86_64

### Version

uv 0.9.0 (39b688653 2025-10-07)

### Python version

Python 3.14.0 (installed via new Python manager)

---

_Label `bug` added by @pfmoore on 2025-10-09 11:32_

---

_Assigned to @konstin by @konstin on 2025-10-09 11:52_

---

_Closed by @konstin on 2025-10-09 14:34_

---

_Comment by @pfmoore on 2025-10-09 15:17_

Thanks!

---
