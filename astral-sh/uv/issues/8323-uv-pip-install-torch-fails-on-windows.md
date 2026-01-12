```yaml
number: 8323
title: "`uv pip install torch` fails on Windows"
type: issue
state: closed
author: jplumail
labels:
  - question
assignees: []
created_at: 2024-10-18T09:55:18Z
updated_at: 2024-10-25T07:23:30Z
url: https://github.com/astral-sh/uv/issues/8323
synced_at: 2026-01-12T15:59:23Z
```

# `uv pip install torch` fails on Windows

---

_@jplumail_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This fails on my windows machine:

```
$ uv venv --python 3.11
Using CPython 3.11.0 interpreter at: C:\Program Files\Python311\python.exe
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate

$ uv pip install torch --index-url https://download.pytorch.org/whl/cu124 -v
DEBUG uv 0.4.24 (b9cd54913 2024-10-17)
DEBUG Searching for default Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.11.0-windows-x86_64-none` at `C:\Users\jeanp\code\tmp\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.11.0 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: torch
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.0
DEBUG Solving with target Python version: >=3.11.0
DEBUG Adding direct dependency: torch*
DEBUG No cache entry for: https://download.pytorch.org/whl/cu124/torch/
DEBUG Searching for a compatible version of torch (*)
DEBUG Selecting: torch==2.5.0+cu124 [compatible] (torch-2.5.0+cu124-cp311-cp311-win_amd64.whl)
DEBUG No cache entry for: https://download.pytorch.org/whl/cu124/torch-2.5.0%2Bcu124-cp311-cp311-win_amd64.whl#sha256=270e004f028b2fc6886388ca01b5f22389f3a7babbd2004a1eec82aa7f669c12
DEBUG Released lock at `C:\Users\jeanp\code\tmp\.venv\.lock`
error: Failed to download `torch==2.5.0+cu124`
  Caused by: Failed to unzip wheel: torch-2.5.0+cu124-cp311-cp311-win_amd64.whl
  Caused by: an upstream reader returned an error: an error occurred during transport: error decoding response body
  Caused by: an error occurred during transport: error decoding response body
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: stream error detected: unspecific protocol error detected
```

```
$ uv --version
uv 0.4.24 (b9cd54913 2024-10-17)
```

---

_Comment by @charliermarsh on 2024-10-23 23:54_

That looks like a transient error. Are you seeing it repeatedly?

---

_Label `question` added by @charliermarsh on 2024-10-23 23:54_

---

_Comment by @jplumail on 2024-10-25 07:23_

I tried again and it works !

I was on my phone wifi when I had the problem. I could install torch from with `uv add` but not from `uv pip` as I remember, that was strange

---

_Closed by @jplumail on 2024-10-25 07:23_

---
