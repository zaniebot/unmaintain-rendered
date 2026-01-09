---
number: 15810
title: C410, C411, and C418 have false positives for calls with extra arguments
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-01-29T14:30:06Z
updated_at: 2025-01-30T23:10:45Z
url: https://github.com/astral-sh/ruff/issues/15810
synced_at: 2026-01-07T13:12:16-06:00
---

# C410, C411, and C418 have false positives for calls with extra arguments

---

_Issue opened by @dscorbett on 2025-01-29 14:30_

### Description

[`unnecessary-literal-within-list-call` (C410)](https://docs.astral.sh/ruff/rules/unnecessary-literal-within-list-call/), [`unnecessary-list-call` (C411)](https://docs.astral.sh/ruff/rules/unnecessary-list-call/), and [`unnecessary-literal-within-dict-call` (C418)](https://docs.astral.sh/ruff/rules/unnecessary-literal-within-dict-call/) have false positives in Ruff 0.9.3 when the `list` or `dict` call has too many positional arguments or (for C411) there is a keyword argument. Their fixes suppress `TypeError`s.
```console
$ cat >c41.py <<'# EOF'
try:
    print(list([1], [2]))
except TypeError as e:
    print(e)
try:
    print(list([x for x in "XYZ"], []))
except TypeError as e:
    print(e)
try:
    print(list([x for x in "XYZ"], foo=[]))
except TypeError as e:
    print(e)
try:
    print(dict({"A": 1}, {"B": 2}))
except TypeError as e:
    print(e)
# EOF

$ python c41.py
list expected at most 1 argument, got 2
list expected at most 1 argument, got 2
list() takes no keyword arguments
dict expected at most 1 argument, got 2

$ ruff --isolated check  --select C410,C411,C418 c41.py --unsafe-fixes --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat c41.py
try:
    print([1])
except TypeError as e:
    print(e)
try:
    print([x for x in "XYZ"])
except TypeError as e:
    print(e)
try:
    print([x for x in "XYZ"])
except TypeError as e:
    print(e)
try:
    print({"A": 1})
except TypeError as e:
    print(e)

$ python c41.py
[1]
['X', 'Y', 'Z']
['X', 'Y', 'Z']
{'A': 1}
```

---

_Label `bug` added by @dylwil3 on 2025-01-30 22:10_

---

_Label `rule` added by @dylwil3 on 2025-01-30 22:10_

---

_Referenced in [astral-sh/ruff#15838](../../astral-sh/ruff/pulls/15838.md) on 2025-01-30 22:15_

---

_Closed by @dylwil3 on 2025-01-30 23:10_

---
