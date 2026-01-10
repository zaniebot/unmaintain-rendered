---
number: 847
title: Parenthesized context managers raise E999
type: issue
state: closed
author: notypecheck
labels:
  - bug
assignees: []
created_at: 2022-11-21T09:33:48Z
updated_at: 2022-12-08T18:39:08Z
url: https://github.com/astral-sh/ruff/issues/847
synced_at: 2026-01-10T01:22:38Z
---

# Parenthesized context managers raise E999

---

_Issue opened by @notypecheck on 2022-11-21 09:33_

Ruff version: 0.0.132
Ruff configuration: Default


Ruff reports E999 (Syntax Error) for parenthesized context managers [that were introduced in python 3.10](https://docs.python.org/3/whatsnew/3.10.html#parenthesized-context-managers)

This raises E999
```
test.py:2:15: E999 SyntaxError: invalid syntax. Got unexpected token 'as'
```
```py
with (
    open("") as a,
    open("") as b,
):
    pass
```

However it works fine if you use `\` to split line:
```py
with open("") as a, \
    open("") as b:
    pass
```


---

_Comment by @g-as on 2022-11-21 13:05_

Related #54, #286 

---

_Label `bug` added by @charliermarsh on 2022-11-21 14:59_

---

_Comment by @charliermarsh on 2022-11-21 15:00_

👍 Unfortunately a known problem that I need to invest in resolving :) Same for structural pattern matching.

---

_Comment by @notypecheck on 2022-11-21 15:20_

Np, just made an issue so it's documented and is easier to find ❤️ 

---

_Comment by @charliermarsh on 2022-11-21 15:27_

Always appreciated!

---

_Comment by @charliermarsh on 2022-12-08 18:39_

Marking as a duplicate of #54.

---

_Closed by @charliermarsh on 2022-12-08 18:39_

---

_Referenced in [astral-sh/ruff#1228](../../astral-sh/ruff/pulls/1228.md) on 2022-12-13 15:08_

---
