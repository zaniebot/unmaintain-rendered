```yaml
number: 6788
title: Rules PT009, SIM300, UP018 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fixes
  - accepted
assignees: []
created_at: 2023-08-22T18:32:03Z
updated_at: 2023-08-23T16:22:06Z
url: https://github.com/astral-sh/ruff/issues/6788
synced_at: 2026-01-10T11:09:49Z
```

# Rules PT009, SIM300, UP018 causes autofix error

---

_Issue opened by @qarmin on 2023-08-22 18:32_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
﻿"""Unit tests for numbers.py."""
class TestNumbers(unittest.TestCase):
        self.assertEqual(1, int(7).denominator)
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_14593204764.py`, the rule codes PT009, SIM300, UP018, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[PY_FILE_TEST_14593204764.py.zip](https://github.com/astral-sh/ruff/files/12411706/PY_FILE_TEST_14593204764.py.zip)



---

_Comment by @zanieb on 2023-08-22 18:33_

```
Got unexpected token 'denominator' at byte offset 88
---
"""Unit tests for numbers.py."""
class TestNumbers(unittest.TestCase):
        assert 7.denominator == 1

---
```

---

_Comment by @charliermarsh on 2023-08-22 18:35_

That's fun.

---

_Comment by @zanieb on 2023-08-22 18:35_

`int(7).denominator` is valid Python

We should avoid stripping the parens

```python
>>> (7).denominator
1
```

```
>>> 7.denominator
  File "<stdin>", line 1
    7.denominator
     ^
SyntaxError: invalid decimal literal
```

Only `UP018` is required to reproduce this.

---

_Label `bug` added by @zanieb on 2023-08-22 19:05_

---

_Label `autofix` added by @zanieb on 2023-08-22 19:05_

---

_Label `accepted` added by @zanieb on 2023-08-22 19:05_

---

_Closed by @zanieb on 2023-08-23 16:22_

---
