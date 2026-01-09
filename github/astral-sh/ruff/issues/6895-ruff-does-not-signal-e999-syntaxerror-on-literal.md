---
number: 6895
title: Ruff does not signal E999 SyntaxError on literal assignment.
type: issue
state: closed
author: 0x6d6e647a
labels:
  - bug
assignees: []
created_at: 2023-08-26T12:28:59Z
updated_at: 2023-11-07T12:16:07Z
url: https://github.com/astral-sh/ruff/issues/6895
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff does not signal E999 SyntaxError on literal assignment.

---

_Issue opened by @0x6d6e647a on 2023-08-26 12:28_

I'm using Ruff v0.0.286 in the following.

For the following Python code:
```python
#!/usr/bin/env python
5 = 3
``` 
No output is generated from `ruff` when analyzed:
```shell
user@host $ ruff literal_assign.py
```

When run from Python:
```shell
user@host $ python literal_assign.py
  File "/home/user/literal_assign.py", line 2
    5 = 3
    ^
SyntaxError: cannot assign to literal here. Maybe you meant '==' instead of '='?
``` 
When analyzed with `flake8`:
```shell
user@host $ flake8 literal_assign.py
literal_assign.py:2:2: E999 SyntaxError: cannot assign to literal here. Maybe you meant '==' instead of '='?
```

---

_Label `bug` added by @charliermarsh on 2023-08-26 21:20_

---

_Comment by @charliermarsh on 2023-08-26 21:20_

Yeah the parser is more permissive than the Python interpreter -- this is better than being _less_ permissive (i.e., rejecting valid code), but it'd be nice to error here.

---

_Referenced in [astral-sh/ruff#7633](../../astral-sh/ruff/issues/7633.md) on 2023-09-25 04:41_

---

_Referenced in [astral-sh/ruff#8524](../../astral-sh/ruff/pulls/8524.md) on 2023-11-06 19:15_

---

_Closed by @BurntSushi on 2023-11-07 12:16_

---
