---
number: 9106
title: False negative for negate-equal-op (SIM201)
type: issue
state: closed
author: last-partizan
labels: []
assignees: []
created_at: 2023-12-12T11:09:05Z
updated_at: 2023-12-12T11:12:07Z
url: https://github.com/astral-sh/ruff/issues/9106
synced_at: 2026-01-10T01:22:48Z
---

# False negative for negate-equal-op (SIM201)

---

_Issue opened by @last-partizan on 2023-12-12 11:09_

I'm using ruff 0.1.7, and in some cases it does not detect SIM201 case.

Take a look at this:

```python
value = 0

if not value == 0:  # Incorrect: No error
    raise ValueError("Invalid data")

if not value == 0:  # Correct: SIM201 [*] Use `value != 0` instead of `not value == 0`
    pass
```

```
> ruff --isolated --select SIM201 /tmp/test.py

/tmp/test.py:6:4: SIM201 [*] Use `value != 0` instead of `not value == 0`
Found 1 error.
```

But there should be two errors.

---

_Comment by @last-partizan on 2023-12-12 11:12_

Oh, that's desired behavior.

Found this https://github.com/astral-sh/ruff/issues/6684, closing.



---

_Closed by @last-partizan on 2023-12-12 11:12_

---
