---
number: 14761
title: SIM300 fix swaps operands with side effects
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-12-03T18:43:24Z
updated_at: 2024-12-30T10:41:35Z
url: https://github.com/astral-sh/ruff/issues/14761
synced_at: 2026-01-07T13:12:16-06:00
---

# SIM300 fix swaps operands with side effects

---

_Issue opened by @dscorbett on 2024-12-03 18:43_

The fix for [`yoda-conditions` (SIM300)](https://docs.astral.sh/ruff/rules/yoda-conditions/) changes the program’s behavior in Ruff 0.8.1 when the two operands have side effects. The fix should be suppressed or marked unsafe when that happens.

This can happen when the first operand is a non-empty dictionary display, which is always marked as probably constant regardless of its contents.

```console
$ cat sim300_1.py
{"": print(1)} == print(2)

$ python sim300_1.py
1
2

$ ruff check --isolated --select SIM300 sim300_1.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim300_1.py
print(2) == {"": print(1)}

$ python sim300_1.py
2
1
```

It can also happen when the first operand is an all-caps attribute.

```console
$ cat sim300_2.py
class C:
    def __getattr__(self, name):
        print(name)
        return name
C().A == print('B')

$ python sim300_2.py
A
B

$ ruff check --isolated --select SIM300 sim300_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim300_2.py
class C:
    def __getattr__(self, name):
        print(name)
        return name
print('B') == C().A

$ python sim300_2.py
B
A
```

There might be other cases.

---

_Label `bug` added by @dylwil3 on 2024-12-04 02:12_

---

_Label `fixes` added by @dylwil3 on 2024-12-04 02:12_

---

_Comment by @charliermarsh on 2024-12-06 02:55_

This is so ridiculous but you are of course right lol.

---

_Referenced in [astral-sh/ruff#15164](../../astral-sh/ruff/pulls/15164.md) on 2024-12-28 03:25_

---

_Closed by @MichaReiser on 2024-12-30 10:41_

---
