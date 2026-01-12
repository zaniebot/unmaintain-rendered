```yaml
number: 15477
title: global python is not in windows path
type: issue
state: open
author: GoFireStar
labels:
  - bug
assignees: []
created_at: 2025-08-24T07:14:29Z
updated_at: 2025-09-02T13:20:10Z
url: https://github.com/astral-sh/uv/issues/15477
synced_at: 2026-01-12T16:02:11Z
```

# global python is not in windows path

---

_@GoFireStar_

### Summary

uv python pin --global 3.11

uv python ensurepath

Executable directory C:\Users\super\.local\bin is already in PATH

where python
D:\DEV\Python\Python310\python.exe # local python
D:\Miniconda3\python.exe # conda python

I can not find global python in python when I want use it by script

result = subprocess.run(['where', 'python'], capture_output=True, text=True, check=False)
if result.returncode == 0 and result.stdout.strip():
return result.stdout.splitlines()[0].strip()

### Platform

windows 10

### Version

uv 0.8.5 (ce37286 2025-08-05)

### Python version

_No response_

---

_Label `bug` added by @GoFireStar on 2025-08-24 07:14_

---

_Comment by @konstin on 2025-08-25 11:03_

In that script, what is the value of `PATH`?

---

_Comment by @RuggedPineapple on 2025-09-02 00:26_

Windows python has a method for handling installations outside of path, py the python launcher.
https://docs.python.org/3/using/windows.html#python-launcher-for-windows

I helped one developer who was unaware it even existed because while it's default in python dot org installs, its excluded from many third party packagers installs, and had accidentally killed an apps functionality in managed domains by changing a call to py into a call to system python. This looks suspiciously like a conflict with user code following windows python best practices by calling py (and py making an unhinted best guess at which python is preferred) and something else expecting all python calls to be to just python.

---

_Comment by @zanieb on 2025-09-02 13:20_

```
uv python pin --global 3.11
uv python ensurepath
```

In this example, you have not installed Python binaries?

I think you want

```
uv python install 3.11 --default
```

---
