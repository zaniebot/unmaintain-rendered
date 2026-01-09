---
number: 3257
title: SIM116 detected twice
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-27T18:45:51Z
updated_at: 2023-02-27T23:08:46Z
url: https://github.com/astral-sh/ruff/issues/3257
synced_at: 2026-01-07T13:12:14-06:00
---

# SIM116 detected twice

---

_Issue opened by @spaceone on 2023-02-27 18:45_

```python
def foo(func_name):
    if func_name == "create":
        return "A"
    elif func_name == "modify":
        return "M"
    elif func_name == "remove":
        return "D"
    elif func_name == "move":
        return "MV"
```

`SIM116` is detected twice, once is enough with highlighting the whole if-block:
```console
$ ruff --show-source foo.py
foo.py:2:5: SIM116 Use a dictionary instead of consecutive `if` statements
  |
2 |       if func_name == "create":
  |  _____^
3 | |         return "A"
4 | |     elif func_name == "modify":
5 | |         return "M"
6 | |     elif func_name == "remove":
7 | |         return "D"
8 | |     elif func_name == "move":
9 | |         return "MV"
  | |___________________^ SIM116
  |

foo.py:4:5: SIM116 Use a dictionary instead of consecutive `if` statements
  |
4 |       elif func_name == "modify":
  |  _____^
5 | |         return "M"
6 | |     elif func_name == "remove":
7 | |         return "D"
8 | |     elif func_name == "move":
9 | |         return "MV"
  | |___________________^ SIM116
  |

Found 2 errors.
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-27 21:38_

---

_Label `bug` added by @charliermarsh on 2023-02-27 22:32_

---

_Referenced in [astral-sh/ruff#3260](../../astral-sh/ruff/pulls/3260.md) on 2023-02-27 23:02_

---

_Closed by @charliermarsh on 2023-02-27 23:08_

---
