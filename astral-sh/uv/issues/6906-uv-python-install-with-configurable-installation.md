---
number: 6906
title: uv python install with configurable installation path
type: issue
state: closed
author: inoa-jboliveira
labels:
  - question
  - configuration
assignees: []
created_at: 2024-09-01T02:46:35Z
updated_at: 2024-09-01T16:55:27Z
url: https://github.com/astral-sh/uv/issues/6906
synced_at: 2026-01-10T01:24:07Z
---

# uv python install with configurable installation path

---

_Issue opened by @inoa-jboliveira on 2024-09-01 02:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Context: 
Any pypy install fails on Windows on paths that contain non ascii characters as noted here https://github.com/pypy/pypy/issues/5000 

So when I do 

```
$ uv run --python pypy -- python --version
error: Querying Python at `C:\Users\JoãoBernardoOliveira\AppData\Roaming\uv\data\python\pypy-3.10.14-windows-x86_64-none\python.exe` failed with exit status exit code: 1
--- stdout:

--- stderr:
ModuleNotFoundError: No module named 'encodings'
debug: OperationError:
debug:  operror-type: ModuleNotFoundError
debug:  operror-value: No module named 'encodings'
---
```
Notice the tilde in the username (this is an account provided by a MS account which does not allow me to pick the actual username).

Since the run command is auto downloading pypy just for the 1st time, it is not the best place to have this config. So my suggestion is something like:

```
$ uv python install pypy@3.10 --path C:\uv\
```

UV will need to be able to find these installs in custom places which could be tricky

---

_Comment by @charliermarsh on 2024-09-01 15:58_

I believe `UV_PYTHON_INSTALL_DIR` works for this -- any issues using that?

---

_Label `question` added by @charliermarsh on 2024-09-01 15:58_

---

_Label `configuration` added by @charliermarsh on 2024-09-01 15:58_

---

_Comment by @inoa-jboliveira on 2024-09-01 16:40_

Sorry, I just searched for config options and could not find one.
It does work, thank you.

Please close this issue

---

_Comment by @charliermarsh on 2024-09-01 16:55_

Thanks for following up, no prob!

---

_Closed by @charliermarsh on 2024-09-01 16:55_

---
