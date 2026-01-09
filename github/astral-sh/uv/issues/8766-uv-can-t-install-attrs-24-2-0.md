---
number: 8766
title: "uv can't install attrs==24.2.0"
type: issue
state: closed
author: sh-shahrokhi
labels:
  - question
assignees: []
created_at: 2024-11-02T15:33:03Z
updated_at: 2024-11-02T15:58:10Z
url: https://github.com/astral-sh/uv/issues/8766
synced_at: 2026-01-07T13:12:18-06:00
---

# uv can't install attrs==24.2.0

---

_Issue opened by @sh-shahrokhi on 2024-11-02 15:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
> uv venv .venv
> & .\.venv\Scripts\activate.ps1
> uv pip install attrs --verbose
```

```
DEBUG uv 0.4.29 (85f9a0d0e 2024-10-30)
DEBUG Searching for default Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.12.7-windows-x86_64-none` at `E:\Projects\st\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.12.7 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: attrs
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: attrs*
DEBUG Found fresh response for: https://pypi.org/simple/attrs/
DEBUG Searching for a compatible version of attrs (*)
DEBUG Selecting: attrs==24.2.0 [compatible] (attrs-24.2.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6a/21/5b6702a7f963e95456c0de2d495f67bf5fd62840ac655dc451586d23d39a/attrs-24.2.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: attrs 1
DEBUG Split specific environment resolution took 0.003s
Resolved 1 package in 4ms
DEBUG Requirement already cached: attrs==24.2.0
DEBUG Released lock at `E:\Projects\st\.venv\.lock`
error: Failed to install: attrs-24.2.0-py3-none-any.whl (attrs==24.2.0)
  Caused by: failed to read directory `E:\.cache\uv\cache\archive-v0\1aFLdX6C1TRczCl0wn9ao`
  Caused by: The system cannot find the path specified. (os error 3)
```


- uv 0.4.29 (85f9a0d0e 2024-10-30)


- Win 11 x64 24H2 




---

_Comment by @zanieb on 2024-11-02 15:34_

Have you tried cleaning the cache with `uv cache clean`?

---

_Comment by @sh-shahrokhi on 2024-11-02 15:39_

Thanks, it worked!

---

_Closed by @sh-shahrokhi on 2024-11-02 15:39_

---

_Label `question` added by @zanieb on 2024-11-02 15:58_

---
