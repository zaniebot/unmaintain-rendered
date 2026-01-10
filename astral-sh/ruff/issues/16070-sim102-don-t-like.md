---
number: 16070
title: "SIM102 don't like :="
type: issue
state: closed
author: xvlady
labels: []
assignees: []
created_at: 2025-02-10T04:35:33Z
updated_at: 2025-02-10T05:02:02Z
url: https://github.com/astral-sh/ruff/issues/16070
synced_at: 2026-01-10T01:22:57Z
---

# SIM102 don't like :=

---

_Issue opened by @xvlady on 2025-02-10 04:35_

### Description

Demo code:
```
def func():
    return {'b': {'c': 'xxx'}}


if __name__ == '__main__':
    if a := func():
        if b := a.get('b'):
            b.get('c')
```
Result check:
```
demo.py:6:5: SIM102 Use a single `if` statement instead of nested `if` statements
  |
5 |   if __name__ == '__main__':
6 |       if a := func():
  |  _____^
7 | |         if b := a.get('b'):
  | |___________________________^ SIM102
8 |               return b.get('c')
  |
```

Ok, edit. Native variant:
```
def func():
    return {'b': {'c': 'xxx'}}


if __name__ == '__main__':
    if a := func() and b := a.get('b'):
            b.get('c')
```

Result:

```
  File "C:\aPrg\WoG.Py\app\reps\jira\rfc\demo.py", line 6
    if a := func() and b := a.get('b'):
                         ^^
SyntaxError: invalid syntax
```

Next variant, kill := is working, but without :=
```
def func():
    return {'b': {'c': 'xxx'}}


if __name__ == '__main__':
    if a := func():
        b = a.get('b')
        if b:
            b.get('c')
```

---

_Comment by @InSyncWithFoo on 2025-02-10 04:46_

`SIM102` itself fixes your example to:

```python
if (a := func()) and (b := a.get('b')):
    b.get('c')
```

As far as I see, this fix is correct. It is marked as unsafe, however.

---

_Closed by @xvlady on 2025-02-10 05:02_

---
