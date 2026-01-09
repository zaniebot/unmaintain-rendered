---
number: 15859
title: UP018 fix ignores operator precedence for numbers with unary operators
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-31T20:51:07Z
updated_at: 2025-02-14T07:42:01Z
url: https://github.com/astral-sh/ruff/issues/15859
synced_at: 2026-01-07T13:12:16-06:00
---

# UP018 fix ignores operator precedence for numbers with unary operators

---

_Issue opened by @dscorbett on 2025-01-31 20:51_

### Description

The fix for [`native-literals` (UP018)](https://docs.astral.sh/ruff/rules/native-literals/) in Ruff 0.9.4 doesn’t take precedence into account for expressions consisting of a unary operator and a numeric literal. Sometimes it ought to insert parentheses.
```console
$ cat >up018.py <<'# EOF'
print(int(-2) ** 2)
print(float(-1.0).imag)
# EOF

$ python up018.py
4
0.0

$ ruff --isolated check --select UP018 up018.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat up018.py
print(-2 ** 2)
print(-1.0.imag)

$ python up018.py
-4
-0.0
```

---

_Label `bug` added by @ntBre on 2025-01-31 22:17_

---

_Label `fixes` added by @MichaReiser on 2025-02-02 10:40_

---

_Referenced in [astral-sh/ruff#15919](../../astral-sh/ruff/pulls/15919.md) on 2025-02-04 00:26_

---

_Closed by @MichaReiser on 2025-02-14 07:42_

---
