---
number: 1790
title: "UP024 fixer replaces wrong members with \"as\" alias import"
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-11T22:31:22Z
updated_at: 2023-01-14T01:39:55Z
url: https://github.com/astral-sh/ruff/issues/1790
synced_at: 2026-01-10T01:22:39Z
---

# UP024 fixer replaces wrong members with "as" alias import

---

_Issue opened by @spaceone on 2023-01-11 22:31_

```
$ ruff --isolated  --select UP foo.py
foo.py:5:7: UP024 Replace aliased errors with `OSError`
foo.py:6:8: UP024 Replace aliased errors with `OSError`
Found 2 error(s).
2 potentially fixable with the --fix option.
$ cat foo.py
from socket import error as SocketError
from events import fire, error

try:
        fire(error())
except SocketError:
        print()
$ ruff --isolated  --select UP --fix foo.py
Found 2 error(s) (2 fixed, 0 remaining).
$ cat foo.py
from socket import error as SocketError
from events import fire, error

try:
        fire(OSError())
except OSError:
        print()
```

1. `fire(error())` must not be replaced
2. the leftover import is now useless

---

_Comment by @charliermarsh on 2023-01-11 22:41_

We need to verify that it's a builtin.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-11 22:41_

---

_Label `autofix` added by @charliermarsh on 2023-01-11 22:41_

---

_Comment by @charliermarsh on 2023-01-11 23:43_

Wait, sorry, what's incorrect here? The unused import will get removed if you enable unused import checks. And this rewrite is correct:

```py
>>> from socket import error as SocketError
>>> SocketError
<class 'OSError'>
```

---

_Comment by @charliermarsh on 2023-01-11 23:43_

Ah, wait, misread the code.

---

_Comment by @charliermarsh on 2023-01-11 23:46_

Though it's not obvious, this is the same underlying issue as #1690. Which is that our import tracking has a few limitations we need to rework.

---

_Referenced in [astral-sh/ruff#1800](../../astral-sh/ruff/pulls/1800.md) on 2023-01-11 23:47_

---

_Referenced in [astral-sh/ruff#1856](../../astral-sh/ruff/pulls/1856.md) on 2023-01-14 01:17_

---

_Closed by @charliermarsh on 2023-01-14 01:39_

---

_Referenced in [astral-sh/ruff#2397](../../astral-sh/ruff/issues/2397.md) on 2023-01-31 14:22_

---
