---
number: 15745
title: PLC2801 fix needs parentheses before subscript or attribute access
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-01-25T23:11:47Z
updated_at: 2025-02-04T09:54:02Z
url: https://github.com/astral-sh/ruff/issues/15745
synced_at: 2026-01-10T01:22:56Z
---

# PLC2801 fix needs parentheses before subscript or attribute access

---

_Issue opened by @dscorbett on 2025-01-25 23:11_

### Description

The fix for [`unnecessary-dunder-call` (PLC2801)](https://docs.astral.sh/ruff/rules/unnecessary-dunder-call/) in Ruff 0.9.3 can change the program’s behavior when the rewritten expression is followed by an attribute access or a subscript access. The fix should wrap the rewritten expression in parentheses in those contexts.
```console
$ cat >plc2801.py <<'#EOF'
print(1j.__add__(1.0).real)
print("x".__add__("y")[0])
#EOF

$ python plc2801.py
1.0
x

$ ruff --isolated check --preview --select PLC2801 plc2801.py --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat plc2801.py
print(1j + 1.0.real)
print("x" + "y"[0])

$ python plc2801.py
(1+1j)
xy
```

---

_Label `bug` added by @dylwil3 on 2025-01-25 23:19_

---

_Label `fixes` added by @dylwil3 on 2025-01-25 23:19_

---

_Label `preview` added by @dylwil3 on 2025-01-25 23:20_

---

_Comment by @mishamsk on 2025-01-26 22:59_

@dylwil3 @AlexWaygood Happy to fix this. I checked the code, there is already a check and logic for wrapping expression in parens, it's just ignoring attribute access, subscript and function call for some reason as lower precedence, which is not correct. Probably a matter of removing 3 lines + updating a test

---

_Comment by @dylwil3 on 2025-01-26 23:10_

Go for it!

---

_Assigned to @mishamsk by @dylwil3 on 2025-01-26 23:11_

---

_Referenced in [astral-sh/ruff#15762](../../astral-sh/ruff/pulls/15762.md) on 2025-01-27 03:07_

---

_Comment by @mishamsk on 2025-01-27 03:08_

> Go for it!

thanks. done #15762 

not exactly 3 lines though 🙈

---

_Closed by @MichaReiser on 2025-02-04 09:54_

---
