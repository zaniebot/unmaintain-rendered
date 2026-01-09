---
number: 14049
title: PLC0208 false positive when a set contains equal values of different types
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-11-01T18:02:54Z
updated_at: 2024-11-03T18:44:53Z
url: https://github.com/astral-sh/ruff/issues/14049
synced_at: 2026-01-07T13:12:16-06:00
---

# PLC0208 false positive when a set contains equal values of different types

---

_Issue opened by @dscorbett on 2024-11-01 18:02_

The fix for [`iteration-over-set` (PLC0208)](https://docs.astral.sh/ruff/rules/iteration-over-set/) has false positives which change behavior. It intentionally ignores sets that contain duplicates, but when detecting duplicates it assumes that literals of different types are never equal. That is not true; for example, `False == 0`, so `{False, 0}` contains a duplicate and is actually equal to `{False}`.

```console
$ ruff --version
ruff 0.7.2

$ cat plc0208.py
for x in {False, 0, 0.0, 0j, True, 1, 1.0}:
    print(x)

$ python plc0208.py 
False
True

$ ruff check --isolated --select PLC0208 plc0208.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat plc0208.py
for x in (False, 0, 0.0, 0j, True, 1, 1.0):
    print(x)

$ python plc0208.py
False
0
0.0
0j
True
1
1.0
```

---

_Comment by @MichaReiser on 2024-11-01 20:37_

Related to https://github.com/astral-sh/ruff/issues/12772

---

_Label `bug` added by @charliermarsh on 2024-11-02 20:17_

---

_Comment by @charliermarsh on 2024-11-02 20:19_

We might want some sort of analogue to `ComparableExpr` that implements these semantics.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-03 18:00_

---

_Referenced in [astral-sh/ruff#14063](../../astral-sh/ruff/pulls/14063.md) on 2024-11-03 18:06_

---

_Closed by @charliermarsh on 2024-11-03 18:44_

---
