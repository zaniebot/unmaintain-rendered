---
number: 10011
title: "Bug(?): FBT001 for a dataclass"
type: issue
state: closed
author: Spectre5
labels:
  - bug
assignees: []
created_at: 2024-02-16T16:46:00Z
updated_at: 2024-02-18T15:03:18Z
url: https://github.com/astral-sh/ruff/issues/10011
synced_at: 2026-01-10T01:22:49Z
---

# Bug(?): FBT001 for a dataclass

---

_Issue opened by @Spectre5 on 2024-02-16 16:46_

In the `__post_init__` method of a dataclass, the InitVar's cannot be passed by keyword argument, they must be positional.  But in this case, FBT001 is trigged.  For example:

```python
from dataclasses import dataclass, InitVar

@dataclass
class Fit:
    force: InitVar[bool] = False

    def __post_init__(self, force: bool) -> None:
        print(force)

Fit(force=True)
```

And then with Ruff:

```bash
$ ruff --version
ruff 0.2.1
$ ruff check --isolated --select=FBT test.py 
test.py:7:29: FBT001 Boolean-typed positional argument in function definition
Found 1 error.
```

If we instead change the definition to `def __post_init__(self, *, force: bool) -> None:`, then FBT001 is no longer triggered, but the code will not work:

```
$ python test.py 
Traceback (most recent call last):
  File "<snip>/test.py", line 11, in <module>
    Fit(True)
  File "<string>", line 3, in __init__
TypeError: Fit.__post_init__() takes 1 positional argument but 2 were given
```

I suggest that dataclasses `__post_init__` be added to an internal whitelist to be ignored by FBT001.

---

_Label `bug` added by @charliermarsh on 2024-02-18 14:22_

---

_Comment by @charliermarsh on 2024-02-18 14:22_

👍 We should allow this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-18 14:56_

---

_Referenced in [astral-sh/ruff#10027](../../astral-sh/ruff/pulls/10027.md) on 2024-02-18 14:56_

---

_Closed by @charliermarsh on 2024-02-18 15:03_

---
