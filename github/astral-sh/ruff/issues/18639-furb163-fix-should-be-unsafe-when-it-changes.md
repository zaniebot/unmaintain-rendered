---
number: 18639
title: FURB163 fix should be unsafe when it changes behavior or deletes comments
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-12T02:35:02Z
updated_at: 2025-06-16T18:13:48Z
url: https://github.com/astral-sh/ruff/issues/18639
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB163 fix should be unsafe when it changes behavior or deletes comments

---

_Issue opened by @dscorbett on 2025-06-12 02:35_

### Summary

The fix for [`redundant-log-base` (FURB163)](https://docs.astral.sh/ruff/rules/redundant-log-base/) should be marked unsafe when it switches the function to `log2` or `log10`, because that can change behavior. It can also change behavior when the first argument is starred. It should also be marked unsafe when it deletes comments.

Floating-point vagaries:
```console
$ cat >furb163_1.py <<'# EOF'
import math
print(math.log(4.13e223, 2))
print(math.log(4.14e223, 10))
# EOF

$ python furb163_1.py
742.8361069415266
223.61700034112087

$ ruff --isolated check furb163_1.py --select FURB163 --fix
Found 2 errors (2 fixed, 0 remaining).

$ python furb163_1.py
742.8361069415265
223.6170003411209
```

Starred arguments:
```console
$ cat >furb163_2.py <<'# EOF'
import math
def print_log(*args):
    try:
        print(math.log(*args, math.e))
    except TypeError as e:
        print(repr(e))
print_log()
print_log(100, 10)
# EOF

$ python furb163_2.py
1.0
TypeError('log expected at most 2 arguments, got 3')

$ ruff --isolated check furb163_2.py --select FURB163 --fix
Found 1 error (1 fixed, 0 remaining).

$ python furb163_2.py
TypeError('log expected at least 1 argument, got 0')
2.0
```

Deleted comments:
```console
$ cat >furb163_3.py <<'# EOF'
import math
print(math.log(
    3,  # This is the argument...
    math.e,  # ... and this is the base.
))
# EOF

$ ruff --isolated check furb163_3.py --select FURB163 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb163_3.py
import math
print(math.log(3))
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-12 03:33_

---

_Label `fixes` added by @ntBre on 2025-06-12 03:33_

---

_Label `help wanted` added by @ntBre on 2025-06-12 03:33_

---

_Referenced in [astral-sh/ruff#18645](../../astral-sh/ruff/pulls/18645.md) on 2025-06-12 11:51_

---

_Closed by @ntBre on 2025-06-16 18:13_

---
