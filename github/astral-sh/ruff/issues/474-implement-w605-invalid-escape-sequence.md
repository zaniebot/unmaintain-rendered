---
number: 474
title: Implement W605, invalid escape sequence
type: issue
state: closed
author: andersk
labels:
  - rule
assignees: []
created_at: 2022-10-26T16:08:00Z
updated_at: 2022-10-26T23:10:24Z
url: https://github.com/astral-sh/ruff/issues/474
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement W605, invalid escape sequence

---

_Issue opened by @andersk on 2022-10-26 16:08_

Python 3.6 [deprecated](https://docs.python.org/3/whatsnew/3.6.html#deprecated-python-behavior) unrecognized backslash escape sequences in string literals such as `"\c"`, and they will eventually become errors. Pycodestyle raises `W605 invalid escape sequence '\c'`. These aren’t adjusted by Black, so it’d be good for Ruff to have this rule too. This probably requires a CST parser (#286).

```console
$ cat a.py
"\c"
$ black a.py
All done! ✨ 🍰 ✨
1 file left unchanged.
$ flake8 a.py
a.py:1:2: W605 invalid escape sequence '\c'
$ python -Wall a.py
/tmp/a.py:1: DeprecationWarning: invalid escape sequence '\c'
  "\c"
$ python -Werror a.py
  File "/tmp/a.py", line 1
    "\c"
    ^^^^
SyntaxError: invalid escape sequence '\c'
$ ruff a.py
No pyproject.toml found.
Falling back to default configuration...
```



---

_Label `rule` added by @charliermarsh on 2022-10-26 16:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-26 21:14_

---

_Comment by @charliermarsh on 2022-10-26 22:30_

I can implement this via token iteration for now.

---

_Referenced in [astral-sh/ruff#482](../../astral-sh/ruff/pulls/482.md) on 2022-10-26 23:09_

---

_Closed by @charliermarsh on 2022-10-26 23:10_

---
