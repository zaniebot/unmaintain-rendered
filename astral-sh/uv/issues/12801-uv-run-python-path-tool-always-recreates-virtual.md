```yaml
number: 12801
title: "`uv run --python <path> <tool>` always recreates virtual environment on Windows"
type: issue
state: open
author: LordAro
labels:
  - bug
  - windows
assignees: []
created_at: 2025-04-10T08:57:18Z
updated_at: 2025-04-10T20:32:35Z
url: https://github.com/astral-sh/uv/issues/12801
synced_at: 2026-01-12T16:01:13Z
```

# `uv run --python <path> <tool>` always recreates virtual environment on Windows

---

_@LordAro_

### Summary

Follow on from #11288 (I know how GH works, comments on closed issues might as well be /dev/null)

I'm still seeing this as an issue with uv 0.6.13 on Windows (MSYS2/UCRT64)

```
$ uv run -vvv --python auxd/python -- pip --version
DEBUG uv 0.6.13 (a0f5c7250 2025-04-07)
DEBUG Found project root: `C:\Users\cpigott\dev\foobar`
DEBUG No workspace root found, using project root
DEBUG Discovered project `foobar` at: C:\Users\cpigott\dev\foobar
TRACE Checking lock for `C:\Users\cpigott\dev\foobar` at `C:\Users\cpigott\AppData\Local\Temp\uv-dcf159be3df1a52e.lock`
DEBUG Acquired lock for `C:\Users\cpigott\dev\foobar`
DEBUG Using Python request `C:/Users/cpigott/dev/foobar/auxd/python` from explicit request
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.12.9, skipping probing: .venv\Scripts\python.exe
DEBUG The virtual environment's Python version does not satisfy `C:/Users/cpigott/dev/foobar/auxd/python`
DEBUG Checking for Python interpreter in directory `auxd/python`
TRACE Cached interpreter info for Python 3.12.9, skipping probing: auxd/python\python.exe
Using CPython 3.12.9 interpreter at: auxd\python\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

.venv/pyvenv.cfg contains `home = C:\Users\cpigott\dev\foobar\auxd\python` as I'd expect

I've tried with and without absolute path on the commandline, no difference

Works fine on Linux though, so maybe something to do with the executables being in different places?

### Platform

Windows 11 (MSYS2/UCRT64)

### Version

uv 0.6.13

### Python version

Python 3.12

---

_Label `bug` added by @LordAro on 2025-04-10 08:57_

---

_Label `windows` added by @charliermarsh on 2025-04-10 14:08_

---

_Assigned to @Gankra by @Gankra on 2025-04-10 20:30_

---

_Comment by @Gankra on 2025-04-10 20:32_

Thanks for refilling, annoying that the previous fix didn't work on windows.

---
