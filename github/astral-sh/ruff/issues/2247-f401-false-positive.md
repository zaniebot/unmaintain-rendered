---
number: 2247
title: F401 false positive
type: issue
state: closed
author: dalcinl
labels:
  - bug
assignees: []
created_at: 2023-01-27T10:19:30Z
updated_at: 2023-01-27T16:25:46Z
url: https://github.com/astral-sh/ruff/issues/2247
synced_at: 2026-01-07T13:12:14-06:00
---

# F401 false positive

---

_Issue opened by @dalcinl on 2023-01-27 10:19_

I have typing stub files like the following minimal reproducer (original code [here](https://github.com/mpi4py/mpi4py/blob/master/src/mpi4py/futures/_core.pyi#L2) ):

```python
# file: stub.pyi
from concurrent.futures import (
    CancelledError as CancelledError,
    TimeoutError as TimeoutError,
)
```
This approach of importing `Foo as Foo` is a common `mypy` convention to mark symbols as "public" or "exported" in the stub file.

However, I'm getting an F401 false positive from `ruff`:
```
$ ruff stub.pyi 
stub.pyi:3:5: F401 `concurrent.futures.TimeoutError` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.
```
Interestingly, the complaint is only about `TimeoutError`, but not `CancelledError`. My wild guess is that it is related to `TimeoutError` being a built-in exception name. To confirm my guess, I wrote the following alternative reproducer:

```
$ cat stub2.pyi 
from pathlib import Path as Path
from builtins import dict as dict
from builtins import list as ListType

$ ruff stub2.pyi 
stub2.pyi:2:22: F401 `builtins.dict` imported but unused
stub2.pyi:3:22: F401 `builtins.list` imported but unused
Found 2 error(s).
2 potentially fixable with the --fix option.
```

Ruff version: `0.0.236`

Sorry I cannot be more helpful, I have zero familiarity with ruff's codebase.

---

_Comment by @charliermarsh on 2023-01-27 12:24_

No worries! Will try to fix today. We do mark those kinds of explicit re-exports as “always used” for this exact reason, so definitely seems like a bug related to built ins.

---

_Label `bug` added by @charliermarsh on 2023-01-27 12:25_

---

_Referenced in [astral-sh/ruff#2251](../../astral-sh/ruff/pulls/2251.md) on 2023-01-27 13:22_

---

_Closed by @charliermarsh on 2023-01-27 16:25_

---
