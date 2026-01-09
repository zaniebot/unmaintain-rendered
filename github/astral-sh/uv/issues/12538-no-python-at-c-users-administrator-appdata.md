---
number: 12538
title: "No Python at '\"C:\\Users\\Administrator\\AppData\\Roaming\\uv\\python\\cpython-3.10.16-windows-x86_64-none\\python.exe'"
type: issue
state: open
author: ThermodynamicBeta
labels:
  - question
assignees: []
created_at: 2025-03-28T23:57:56Z
updated_at: 2025-04-01T17:24:58Z
url: https://github.com/astral-sh/uv/issues/12538
synced_at: 2026-01-07T13:12:18-06:00
---

# No Python at '"C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.10.16-windows-x86_64-none\python.exe'

---

_Issue opened by @ThermodynamicBeta on 2025-03-28 23:57_

### Summary

I am writing an ansible playbook to install uv and set up a venv (uv venv --seed --python 3.10) on Windows 10 and 11 computers 

uv gets installed as Administrator (via scoop), the virtual environment gets created as Administrator too.

After the playbook runs, I go onto the computer as non-admin and activate the virtual environment (.\\.venv\Scripts\activate).

Running any python command (pip, pytest, python) yields 
**`No Python at '"C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.10.16-windows-x86_64-none\python.exe'`**

Navigating to that directory, it is actually there.

Even running the full path to .\.venv\Scripts\python.exe yields the same error.

Running uv run <stuff> does give more info:
**"Querying Python at <venv path> failed with exit status exit code: 103"**

Logically this would be because of administrator privileges, but I haven't tried reproducing this manually to confirm that.

My questions are:

1. I thought that uv didn't make symlinks on Windows? Why does the error talk about the Administrator python.exe instead of the venv .exe?
2. What are my options for fixing this?




### Platform

Windows 10, Windows 11

### Version

uv 0.6.10 (f2a2d982b 2025-03-25)

### Python version

3.10.16

---

_Label `bug` added by @ThermodynamicBeta on 2025-03-28 23:57_

---

_Comment by @ThermodynamicBeta on 2025-03-31 16:58_

Additional data point, opening Admin powershell and activating the virtual environment, Python commands work as expected. I'm wondering if there's a way to install the managed python version into a place where every user can use it?

---

_Comment by @ThermodynamicBeta on 2025-04-01 16:52_

It seems like the best solution for me was to set the UV_PYTHON_INSTALL_DIR env variable to something outside the Administrator directory. Might also need to set ACL for that new directory.

---

_Comment by @zanieb on 2025-04-01 17:00_

Yeah this seems like a weird permissions problem. We might be able to improve that error case? But I think you'll need to set up the permissions correctly on your end.

---

_Label `bug` removed by @zanieb on 2025-04-01 17:00_

---

_Label `question` added by @zanieb on 2025-04-01 17:00_

---

_Comment by @ThermodynamicBeta on 2025-04-01 17:03_

I do notice that if I delete the interpreter after its working, running the python or pip command creates the same error. This goes back to the symlink question, why isn't it a new copy in the virtual environment, isn't that the point?

---

_Comment by @zanieb on 2025-04-01 17:23_

Virtual environments are "virtual", they usually just have a launcher that forwards to the real interpreter.

In fact, that error message is from the CPython virtual environment launcher: https://github.com/python/cpython/blob/84be5244de75c92904fb41326c9a69f19051e7ab/PC/launcher.c#L1954

---

_Comment by @ThermodynamicBeta on 2025-04-01 17:24_

Oh, TIL, thanks

---

_Referenced in [astral-sh/uv#14814](../../astral-sh/uv/issues/14814.md) on 2025-07-22 15:59_

---
