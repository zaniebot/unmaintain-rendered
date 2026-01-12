```yaml
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
synced_at: 2026-01-12T15:54:44Z
```

# Incorrect: Local variable assigned but never used.

---

_@chebee7i_

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

_Closed by @charliermarsh on 2023-05-24 14:10_

---
