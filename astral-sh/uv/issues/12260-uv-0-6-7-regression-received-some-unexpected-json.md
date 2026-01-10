---
number: 12260
title: "uv 0.6.7 regression \"Received some unexpected JSON\" ... \"expected a borrowed string\" when installing \"pyenchant==3.3.0rc1\""
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2025-03-18T03:46:09Z
updated_at: 2025-03-18T13:27:23Z
url: https://github.com/astral-sh/uv/issues/12260
synced_at: 2026-01-10T01:25:17Z
---

# uv 0.6.7 regression "Received some unexpected JSON" ... "expected a borrowed string" when installing "pyenchant==3.3.0rc1"

---

_Issue opened by @adamtheturtle on 2025-03-18 03:46_

### Summary

Command:

```
uv pip install --reinstall "pyenchant==3.3.0rc1"
```

With `uv 0.6.6` I can install `"pyenchant==3.3.0rc1"`

With `uv 0.6.7` I get:

```
⠙ Resolving dependencies...                                                                                                                                                 error: Received some unexpected JSON from https://pypi.org/simple/pyenchant/
  Caused by: invalid type: string ">=\"3.5\"", expected a borrowed string at line 1 column 50181
```

### Platform

macOS 15.3, and GitHub Actions Ubuntu

### Version

0.6.7

### Python version

Python 3.11 / 3.12 at least

---

_Label `bug` added by @adamtheturtle on 2025-03-18 03:46_

---

_Comment by @shauneccles on 2025-03-18 05:03_

```
(test-venv) PS C:\Users\shaun\test-venv> uv -vv pip install --reinstall "pyenchant==3.3.0rc1"
DEBUG uv 0.6.7 (029b9e1fc 2025-03-17)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at C:\Users\shaun\test-venv\.venv\Scripts\python.exe
DEBUG Found `cpython-3.13.1-windows-x86_64-none` at `C:\Users\shaun\test-venv\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.13.1 environment at: .venv
TRACE Checking lock for `.venv` at `.venv\.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13.1
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: pyenchant>=3.3.0rc1, <3.3.0rc1+
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: pyenchant. remaining choices:
TRACE Fetching metadata for pyenchant from https://pypi.org/simple/pyenchant/
TRACE Checking lock for `C:\Users\shaun\AppData\Local\uv\cache\simple-v15\pypi\pyenchant.lock` at `C:\Users\shaun\AppData\Local\uv\cache\simple-v15\pypi\pyenchant.lock`
DEBUG Acquired lock for `C:\Users\shaun\AppData\Local\uv\cache\simple-v15\pypi\pyenchant.lock`
TRACE No cache entry exists for C:\Users\shaun\AppData\Local\uv\cache\simple-v15\pypi\pyenchant.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pyenchant/
TRACE Sending fresh GET request for https://pypi.org/simple/pyenchant/
TRACE Handling request for https://pypi.org/simple/pyenchant/
TRACE Request for https://pypi.org/simple/pyenchant/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pyenchant/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pyenchant/
TRACE Cached request https://pypi.org/simple/pyenchant/ is storable because its response has a 'public' cache-control directive
TRACE Considering retry of error: Error { kind: BadJson { source: Error("invalid type: string \">=\\\"3.5\\\"\", expected a borrowed string", line: 1, column: 50181), url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple/pyenchant/", query: None, fragment: None } } }
TRACE Cannot retry error: not an IO error
DEBUG Released lock at `C:\Users\shaun\AppData\Local\uv\cache\simple-v15\pypi\pyenchant.lock`
DEBUG Released lock at `C:\Users\shaun\test-venv\.venv\.lock`
error: Received some unexpected JSON from https://pypi.org/simple/pyenchant/
  Caused by: invalid type: string ">=\"3.5\"", expected a borrowed string at line 1 column 50181
(test-venv) PS C:\Users\shaun\test-venv>
```

---

_Comment by @alberto-gomezcasado-AP on 2025-03-18 11:35_

I am having the same problem installing tbump, using uv-0.6.7-py3-none-win_amd64.whl

```
nox > Running session bump
nox > Creating virtual environment (uv) using python.exe in .nox\bump
nox > git describe --tags --abbrev=0
nox > uv pip install tbump
nox > Command uv pip install tbump failed with exit code 2:
Using Python 3.9.4 environment at: .nox\bump
error: Received some unexpected JSON from https://pypi.org/simple/tbump/
  Caused by: invalid type: string ">=\"3.5\"", expected a borrowed string at line 1 column 19557
nox > Session bump failed.
```



---

_Referenced in [pyca/cryptography#12633](../../pyca/cryptography/pulls/12633.md) on 2025-03-18 12:15_

---

_Comment by @charliermarsh on 2025-03-18 12:28_

Apologies, I’ll fix it this morning.

---

_Comment by @alex on 2025-03-18 12:29_

Thanks Charlie!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-18 13:01_

---

_Referenced in [astral-sh/uv#12278](../../astral-sh/uv/pulls/12278.md) on 2025-03-18 13:09_

---

_Comment by @charliermarsh on 2025-03-18 13:09_

Fixed in https://github.com/astral-sh/uv/pull/12278.

---

_Closed by @charliermarsh on 2025-03-18 13:27_

---

_Referenced in [pypa/hatch#1939](../../pypa/hatch/issues/1939.md) on 2025-03-18 18:51_

---
