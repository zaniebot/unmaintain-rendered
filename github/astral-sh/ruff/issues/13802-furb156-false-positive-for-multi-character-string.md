---
number: 13802
title: "FURB156 false positive for multi-character string before `in`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - preview
assignees: []
created_at: 2024-10-17T20:27:37Z
updated_at: 2024-11-09T20:31:28Z
url: https://github.com/astral-sh/ruff/issues/13802
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB156 false positive for multi-character string before `in`

---

_Issue opened by @dscorbett on 2024-10-17 20:27_

[`hardcoded-string-charset` (FURB156)](https://docs.astral.sh/ruff/rules/hardcoded-string-charset/) has a false positive with an incorrect fix. Given an expression of the form `<expression> in <string literal>`, the rule tries to replace the string literal with a variable from the `string` module, ignoring the order of the characters in the string literal. That is only valid when the left-hand-side expression is a string of length 1. If it is multiple characters, reordering the characters in the right-hand-side expression can change the value of the `in` operation.

```console
$ ruff --version
ruff 0.7.0

$ cat furb156.py
print("89" in "9876543210")

$ python furb156.py
False

$ ruff check --isolated --preview --select FURB156 furb156.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb156.py
import string
print("89" in string.digits)

$ python furb156.py
True
```

---

_Label `bug` added by @AlexWaygood on 2024-10-17 23:58_

---

_Label `preview` added by @MichaReiser on 2024-10-18 06:19_

---

_Referenced in [astral-sh/ruff#13820](../../astral-sh/ruff/pulls/13820.md) on 2024-10-19 13:49_

---

_Comment by @Aditya-PS-05 on 2024-10-19 17:21_

@MichaReiser , 
Please review it

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 20:07_

---

_Referenced in [astral-sh/ruff#14233](../../astral-sh/ruff/pulls/14233.md) on 2024-11-09 20:08_

---

_Closed by @charliermarsh on 2024-11-09 20:31_

---

_Referenced in [astral-sh/ruff#21144](../../astral-sh/ruff/issues/21144.md) on 2025-11-01 02:08_

---

_Referenced in [astral-sh/ruff#21181](../../astral-sh/ruff/pulls/21181.md) on 2025-11-01 02:22_

---
