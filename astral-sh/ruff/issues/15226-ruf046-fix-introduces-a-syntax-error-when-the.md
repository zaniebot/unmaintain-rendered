```yaml
number: 15226
title: RUF046 fix introduces a syntax error when the cast expression contains a newline
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-02T18:18:43Z
updated_at: 2025-01-02T22:43:17Z
url: https://github.com/astral-sh/ruff/issues/15226
synced_at: 2026-01-12T15:54:54Z
```

# RUF046 fix introduces a syntax error when the cast expression contains a newline

---

_@dscorbett_

The fix for [`unnecessary-cast-to-int` (RUF046)](https://docs.astral.sh/ruff/rules/unnecessary-cast-to-int/) can crash Ruff in version 0.8.5 or change behavior when the cast expression contains a newline.

Here is a crash:
```console
$ cat ruf046_1.py
int(1
& 1)

$ ruff check --isolated --preview --select RUF046 ruf046_1.py --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `ruf046_1.py`, the rule codes RUF046, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

Here is a syntax error:
```console
$ cat ruf046_2.py
int(1
* 1)

$ ruff check --isolated --preview --select RUF046 ruf046_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf046_2.py
1
* 1

$ python ruf046_2.py
  File "ruf046_2.py", line 2
    * 1
    ^^^
SyntaxError: can't use starred expression here
```

Here is a runtime behavior change:
```console
$ cat ruf046_3.py
x = int(1
+ 1)
print(x)

$ python ruf046_3.py
2

$ ruff check --isolated --preview --select RUF046 ruf046_3.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf046_3.py
x = 1
+ 1
print(x)

$ python ruf046_3.py
1
```

---

_Label `bug` added by @charliermarsh on 2025-01-02 20:33_

---

_Comment by @charliermarsh on 2025-01-02 20:33_

Thanks!

---

_Label `fixes` added by @charliermarsh on 2025-01-02 20:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-02 20:50_

---

_Closed by @charliermarsh on 2025-01-02 22:43_

---

_Closed by @charliermarsh on 2025-01-02 22:43_

---
