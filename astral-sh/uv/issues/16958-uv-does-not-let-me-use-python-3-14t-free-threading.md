---
number: 16958
title: "`UV` Does not let me use python 3.14t (Free threading )"
type: issue
state: closed
author: Vedant-Bisen
labels:
  - bug
assignees: []
created_at: 2025-12-03T07:48:10Z
updated_at: 2025-12-03T09:03:40Z
url: https://github.com/astral-sh/uv/issues/16958
synced_at: 2026-01-10T01:26:11Z
---

# `UV` Does not let me use python 3.14t (Free threading )

---

_Issue opened by @Vedant-Bisen on 2025-12-03 07:48_

### Summary

I was trying to test the new 3.14 Free threading feature and built an env using `uv venv -p3.14t`It perfectly downloaded the version and created env
When i ran the code using `uv run main.py` it removed the env and recreated one without freethreading 
```
(FreeThreadingTest) ttdf@TTDFs-Mac-mini FreeThreadingTest % uv run main.py
Using CPython 3.14.0rc1
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Execution time: 1.64 seconds
```

I tried multiple times but it keeps doing that 

### Platform

macOS 15.6

### Version

uv 0.8.10

### Python version

python3.14

---

_Label `bug` added by @Vedant-Bisen on 2025-12-03 07:48_

---

_Comment by @DhavalGojiya on 2025-12-03 08:16_

Can you run this command again with
`uv run -v main.py`

---

_Comment by @konstin on 2025-12-03 08:24_

Do you have a `.python_version` file and/or a `project.requires-python` value in `pyproject.toml`?

---

_Comment by @Vedant-Bisen on 2025-12-03 08:56_

> Do you have a `.python_version` file and/or a `project.requires-python` value in `pyproject.toml`?

Yes, I have pyproject.toml, .python-version as well as uv.lock 
all three have `requires-python = ">=3.14" `


---

_Comment by @Vedant-Bisen on 2025-12-03 08:58_

> Can you run this command again with `uv run -v main.py`

```
(FreeThreadingTest) ttdf@TTDFs-Mac-mini FreeThreadingTest % uv run -v main.py
DEBUG uv 0.8.10 (7e817bafd 2025-08-13)
DEBUG Found project root: `/Users/ttdf/Documents/FreeThreadingTest`
DEBUG No workspace root found, using project root
DEBUG Discovered project `freethreadingtest` at: /Users/ttdf/Documents/FreeThreadingTest
DEBUG Acquired lock for `/Users/ttdf/Documents/FreeThreadingTest`
DEBUG Reading Python requests from version file at `/Users/ttdf/Documents/FreeThreadingTest/.python-version`
DEBUG Using Python request `3.14` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version does not satisfy the request: `Python 3.14`
DEBUG Searching for Python 3.14 in managed installations or search path
DEBUG Searching for managed installations at `/Users/ttdf/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.14.0rc1-macos-aarch64-none`
DEBUG Found `cpython-3.14.0rc1-macos-aarch64-none` at `/Users/ttdf/.local/share/uv/python/cpython-3.14.0rc1-macos-aarch64-none/bin/python3.14` (managed installations)
DEBUG Skipping pre-release installation cpython-3.14.0rc1-macos-aarch64-none
DEBUG Found managed installation `cpython-3.14.0rc1+freethreaded-macos-aarch64-none`
DEBUG Found `cpython-3.14.0rc1+freethreaded-macos-aarch64-none` at `/Users/ttdf/.local/share/uv/python/cpython-3.14.0rc1+freethreaded-macos-aarch64-none/bin/python3.14t` (managed installations)
DEBUG Skipping interpreter at `/Users/ttdf/.local/share/uv/python/cpython-3.14.0rc1+freethreaded-macos-aarch64-none/bin/python3.14t` from managed installations: does not satisfy request `3.14`
DEBUG Skipping managed installation `cpython-3.12.11-macos-aarch64-none`: does not satisfy `3.14`
DEBUG Found `cpython-3.14.0rc1+freethreaded-macos-aarch64-none` at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python3.14` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python3.14`: system interpreter required
DEBUG Found `cpython-3.14.0rc1+freethreaded-macos-aarch64-none` at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.14.0rc1+freethreaded-macos-aarch64-none` at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/opt/homebrew/opt/python@3.13/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `3.14`
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/opt/homebrew/opt/python@3.13/libexec/bin/python` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `3.14`
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/Library/Developer/CommandLineTools/usr/bin/python3` from search path: does not satisfy request `3.14`
DEBUG Found `cpython-3.13.7-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from search path: does not satisfy request `3.14`
DEBUG Allowing pre-release installation cpython-3.14.0rc1-macos-aarch64-none: no stable installations
Using CPython 3.14.0rc1
Removed virtual environment at: .venv
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/ttdf/.local/share/uv/python/cpython-3.14.0rc1-macos-aarch64-none/bin/python3.14
DEBUG Using base executable for virtual environment: /Users/ttdf/.local/share/uv/python/cpython-3.14.0rc1-macos-aarch64-none/bin/python3.14
DEBUG Released lock at `/var/folders/4_/tz43064s6g3fr7sbttsyvz040000gn/T/uv-a27ead4115c52ea8.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: freethreadingtest @ file:///Users/ttdf/Documents/FreeThreadingTest
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 2ms
DEBUG Stripping pre-release from `python_full_version`: 3.14.0rc1
DEBUG Using request timeout of 30s
Audited in 0.00ms
DEBUG Released lock at `/Users/ttdf/Documents/FreeThreadingTest/.venv/.lock`
DEBUG Using Python 3.14.0rc1 interpreter at: /Users/ttdf/Documents/FreeThreadingTest/.venv/bin/python
DEBUG Running `python main.py`
DEBUG Spawned child 40684 in process group 40682
Execution time: 1.69 seconds
DEBUG Command exited with code: 0
(FreeThreadingTest) ttdf@TTDFs-Mac-mini FreeThreadingTest %
```

Hope this helps

---

_Comment by @Vedant-Bisen on 2025-12-03 09:03_

Found the fix, the issue raised from the `.python-version file. `
The file needed to have `3.14t` mentioned and not `3.14` as that would fail, causing it to rebuild the venv.  

---

_Closed by @Vedant-Bisen on 2025-12-03 09:03_

---
