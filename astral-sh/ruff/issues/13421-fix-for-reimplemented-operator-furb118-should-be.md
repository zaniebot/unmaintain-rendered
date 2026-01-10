```yaml
number: 13421
title: "Fix for `reimplemented-operator` (FURB118) should be marked unsafe"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-09-20T15:54:14Z
updated_at: 2024-10-03T21:39:24Z
url: https://github.com/astral-sh/ruff/issues/13421
synced_at: 2026-01-10T11:09:55Z
```

# Fix for `reimplemented-operator` (FURB118) should be marked unsafe

---

_Issue opened by @dscorbett on 2024-09-20 15:54_

The fix for [`reimplemented-operator` (FURB118)](https://docs.astral.sh/ruff/rules/reimplemented-operator/) should be marked unsafe because the code could rely on the parameter names. Compare [`unnecessary-lambda` (PLW0108)](https://docs.astral.sh/ruff/rules/unnecessary-lambda/), whose fix is already marked unsafe for that reason. There are some cases where it can be proved that the function is never called with keyword arguments so the fix is safe, but in general that is not possible.

```console
$ ruff --version
ruff 0.6.6
$ cat furb118.py
add = lambda x, y: x + y
print(add(x=1, y=2))
$ python furb118.py
3
$ ruff check --isolated --preview --select FURB118 furb118.py --fix
Found 1 error (1 fixed, 0 remaining).
$ python furb118.py 2>&1 | tail -n 3
    print(add(x=1, y=2))
          ^^^^^^^^^^^^^
TypeError: _operator.add() takes no keyword arguments
```

---

_Label `bug` added by @AlexWaygood on 2024-09-21 20:31_

---

_Label `fixes` added by @AlexWaygood on 2024-09-21 20:31_

---

_Label `help wanted` added by @AlexWaygood on 2024-09-21 20:31_

---

_Closed by @zanieb on 2024-10-03 21:39_

---

_Closed by @zanieb on 2024-10-03 21:39_

---
