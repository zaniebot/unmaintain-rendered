---
number: 9543
title: "Cache behaves differently with and without cache (`INP001`)"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2024-01-16T06:49:32Z
updated_at: 2024-01-16T22:49:33Z
url: https://github.com/astral-sh/ruff/issues/9543
synced_at: 2026-01-07T13:12:15-06:00
---

# Cache behaves differently with and without cache (`INP001`)

---

_Issue opened by @bersbersbers on 2024-01-16 06:49_

`INP001` is issued after creating `__init__.py`, due to caching and potentially due to `N999`:

```
$ tree /f
C:.
├───example
│       generate-example.py
│
└───project
        main.py
        __init__.py

$ ruff --no-cache --isolated --select INP,N .
example\generate-example.py:1:1: INP001 File `example\generate-example.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.

$ ruff --isolated --select INP,N .
example\generate-example.py:1:1: INP001 File `example\generate-example.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.

$ echo. > example\__init__.py

$ tree /f
C:.
├───example
│       generate-example.py
│       __init__.py
│
└───project
        main.py
        __init__.py

$ ruff --no-cache --isolated --select INP,N .
example\generate-example.py:1:1: N999 Invalid module name: 'generate-example'
Found 1 error.

(So far, so good)

$ ruff --isolated --select INP,N .
example\generate-example.py:1:1: INP001 File `example\generate-example.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.

$ ruff --isolated --select INP,N . --fix
example\generate-example.py:1:1: N999 Invalid module name: 'generate-example'
Found 1 error.

$ ruff --isolated --select INP,N .
example\generate-example.py:1:1: INP001 File `example\generate-example.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.

$ ruff --version
ruff 0.1.13
```


---

_Comment by @charliermarsh on 2024-01-16 19:36_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/5449. (I think `--fix` is helping because we avoid reading the cache when applying fixes.)

---

_Closed by @charliermarsh on 2024-01-16 19:36_

---

_Label `bug` added by @charliermarsh on 2024-01-16 19:36_

---

_Comment by @bersbersbers on 2024-01-16 22:34_

Thanks, and sorry for the duplicate! I should have found that one.

---

_Comment by @charliermarsh on 2024-01-16 22:49_

All good, no worries!

---
