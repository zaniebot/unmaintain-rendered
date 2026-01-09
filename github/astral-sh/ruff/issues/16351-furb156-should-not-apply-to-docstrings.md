---
number: 16351
title: FURB156 should not apply to docstrings
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-24T18:18:57Z
updated_at: 2025-02-26T16:30:15Z
url: https://github.com/astral-sh/ruff/issues/16351
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB156 should not apply to docstrings

---

_Issue opened by @dscorbett on 2025-02-24 18:18_

### Description

[`hardcoded-string-charset` (FURB156)](https://docs.astral.sh/ruff/rules/hardcoded-string-charset/) applies to docstrings in Ruff 0.9.7, but it should not.

When the string is a module docstring, the fix inserts the import after its use, causing a `NameError`.
```console
$ cat >furb156_1.py <<'# EOF'
"01234567"
print(__doc__)
# EOF

$ python furb156_1.py
01234567

$ ruff --isolated check --preview --select FURB156 furb156_1.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb156_1.py
string.octdigits
import string
print(__doc__)

$ python furb156_1.py 
Traceback (most recent call last):
  File "furb156_1.py", line 1, in <module>
    string.octdigits
    ^^^^^^
NameError: name 'string' is not defined. Did you forget to import 'string'?
```

It can also introduce a syntax error.
```console
$ printf '"01234567"'  | ruff --isolated check --preview --select FURB156 - --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

Even when the import is inserted in the intended place, FURB156 should not apply to docstrings, because replacing them with identifiers makes them not be docstrings.
```console
$ cat >furb156_2.py <<'# EOF'
class C:
    "01234567"
print(C.__doc__)
# EOF

$ python furb156_2.py
01234567

$ ruff --isolated check --preview --select FURB156 furb156_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb156_2.py
import string
class C:
    string.octdigits
print(C.__doc__)

$ python furb156_2.py
None
```

---

_Label `bug` added by @ntBre on 2025-02-24 21:36_

---

_Label `help wanted` added by @MichaReiser on 2025-02-25 08:47_

---

_Comment by @VascoSch92 on 2025-02-25 15:58_

I can give a try to that if needed ;-) 

---

_Assigned to @VascoSch92 by @MichaReiser on 2025-02-25 16:40_

---

_Referenced in [astral-sh/ruff#16391](../../astral-sh/ruff/pulls/16391.md) on 2025-02-26 12:57_

---

_Closed by @MichaReiser on 2025-02-26 16:30_

---
