---
number: 4623
title: "Incorrect: Local variable assigned but never used."
type: issue
state: closed
author: chebee7i
labels:
  - bug
assignees: []
created_at: 2023-05-24T10:47:06Z
updated_at: 2023-05-24T14:10:17Z
url: https://github.com/astral-sh/ruff/issues/4623
synced_at: 2026-01-07T13:12:14-06:00
---

# Incorrect: Local variable assigned but never used.

---

_Issue opened by @chebee7i on 2023-05-24 10:47_

```console
$ python --version
Python 3.8.15

$ ruff --version
ruff 0.0.265

$ cat demo.py
from typing import NewType


def func():
    name = 'x'
    NT = NewType(name, int)

    return NT

$ ruff demo.py
demo.py:5:5: F841 [*] Local variable `name` is assigned to but never used

```

No `pyproject.toml` or `ruff.toml`


---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-24 13:34_

---

_Label `bug` added by @charliermarsh on 2023-05-24 13:34_

---

_Referenced in [astral-sh/ruff#4627](../../astral-sh/ruff/pulls/4627.md) on 2023-05-24 13:40_

---

_Closed by @charliermarsh on 2023-05-24 14:10_

---
