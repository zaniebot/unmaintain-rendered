```yaml
number: 11263
title: "`PLW1514` does not check for uses of `pathlib.Path.open()` without an explicit encoding argument."
type: issue
state: closed
author: yuji38kwmt
labels:
  - good first issue
  - rule
assignees: []
created_at: 2024-05-03T13:58:18Z
updated_at: 2024-05-09T12:36:22Z
url: https://github.com/astral-sh/ruff/issues/11263
synced_at: 2026-01-10T11:09:53Z
```

# `PLW1514` does not check for uses of `pathlib.Path.open()` without an explicit encoding argument.

---

_Issue opened by @yuji38kwmt on 2024-05-03 13:58_


# Description

[PLW1514](https://docs.astral.sh/ruff/rules/unspecified-encoding/) (`unspecified-encoding`) does not check for uses of `pathlib.Path.open()` without an explicit encoding argument.
But with Pylint, [W1514](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/unspecified-encoding.html) checks.

I expect Ruff to behave in the same way as Pylint.

### sample.py

```python
from pathlib import Path

with open("foo.txt") as f:
    f.readline()


with Path("foo.txt").open() as f:
    f.readline()
```

### Checks with Ruff

```
$ ruff --version
ruff 0.4.2

$ ruff check --select PLW1514 --preview --output-format=concise --isolated
sample.py:3:6: PLW1514 `open` in text mode without explicit `encoding` argument
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

### Checks with Pylint

```
$ pylint --version
pylint 3.1.0
astroid 3.1.0
Python 3.12.1 (main, Feb  7 2024, 10:25:21) [GCC 11.4.0]

$ pylint sample.py --enable W1514 --disable C
************* Module sample
sample.py:3:5: W1514: Using open without explicitly specifying an encoding (unspecified-encoding)
sample.py:7:5: W1514: Using open without explicitly specifying an encoding (unspecified-encoding)

------------------------------------------------------------------
Your code has been rated at 6.00/10 (previous run: 4.00/10, +2.00)
```



---

_Label `rule` added by @zanieb on 2024-05-03 14:06_

---

_Comment by @zanieb on 2024-05-03 14:06_

Sounds good to me!

---

_Label `good first issue` added by @zanieb on 2024-05-03 14:06_

---

_Comment by @augustelalande on 2024-05-04 22:39_

I'll implement this

---

_Assigned to @augustelalande by @dhruvmanila on 2024-05-06 08:43_

---

_Closed by @dhruvmanila on 2024-05-09 12:36_

---
