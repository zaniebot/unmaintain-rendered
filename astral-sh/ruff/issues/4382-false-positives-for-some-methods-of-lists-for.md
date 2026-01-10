---
number: 4382
title: False positives for some methods of lists for boolean trap rule FBT003
type: issue
state: closed
author: namurphy
labels: []
assignees: []
created_at: 2023-05-11T21:23:36Z
updated_at: 2023-05-11T22:00:56Z
url: https://github.com/astral-sh/ruff/issues/4382
synced_at: 2026-01-10T01:22:43Z
---

# False positives for some methods of lists for boolean trap rule FBT003

---

_Issue opened by @namurphy on 2023-05-11 21:23_

When providing `True` or `False` as an argument to the `append`, `count`, `insert`, and `remove` methods of a `list`, the rule for catching boolean traps (`FBT003`) yields false positives even though the arguments are positional-only.

I created `file.py` which contains:
```python
some_list = []
some_list.append(True)
some_list.count(True)
some_list.insert(0, True)
some_list.remove(True)
```
Using ruff `v0.0.265`, I ran:
```bash
ruff file.py --select FBT003 --isolated
```
and got the output:
```
file.py:2:18: FBT003 Boolean positional value in function call
file.py:3:17: FBT003 Boolean positional value in function call
file.py:4:21: FBT003 Boolean positional value in function call
file.py:5:18: FBT003 Boolean positional value in function call
```
This follows up on #1336.

Thank you!


---

_Referenced in [astral-sh/ruff#4385](../../astral-sh/ruff/pulls/4385.md) on 2023-05-11 21:49_

---

_Closed by @charliermarsh on 2023-05-11 21:55_

---

_Comment by @namurphy on 2023-05-11 21:58_

Wow, that was quick!  Thank you for fixing it!

---

_Comment by @charliermarsh on 2023-05-11 22:00_

No prob, it's an easy one :)

---

_Referenced in [astral-sh/ruff#10485](../../astral-sh/ruff/issues/10485.md) on 2024-03-20 14:38_

---
