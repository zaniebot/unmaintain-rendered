---
number: 7100
title: Rules UP021 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:39:52Z
updated_at: 2023-09-03T21:17:05Z
url: https://github.com/astral-sh/ruff/issues/7100
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules UP021 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:39_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP021 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
import subprocess
def _run_pocess(params:list) -> schemas.ProcessResult:
    p = subprocess.run(
        text=True, 
        universal_newlines=True, 
    )
```

error
```
/home/rafal/test/tmp_folder/3648563.py:5:9: UP021 `universal_newlines` is deprecated, use `text`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/3648563.py`, the rule codes UP021, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506888/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 20:58_

---

_Label `bug` added by @charliermarsh on 2023-09-03 20:58_

---

_Referenced in [astral-sh/ruff#7112](../../astral-sh/ruff/pulls/7112.md) on 2023-09-03 21:00_

---

_Closed by @charliermarsh on 2023-09-03 21:17_

---
