```yaml
number: 18209
title: "SIM105 fix changes behavior for `except ():` and `except:`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-19T23:59:26Z
updated_at: 2025-06-23T14:01:59Z
url: https://github.com/astral-sh/ruff/issues/18209
synced_at: 2026-01-12T15:54:56Z
```

# SIM105 fix changes behavior for `except ():` and `except:`

---

_@dscorbett_

### Summary

The fix for [`suppressible-exception` (SIM105)](https://docs.astral.sh/ruff/rules/suppressible-exception/) changes behavior when the `except` tuple is empty. Instead of `contextlib.suppress(Exception)`, the replacement should be `contextlib.suppress()`. Alternatively, the rule (or just the fix) could be suppressed, letting [`except-with-empty-tuple` (B029)](https://docs.astral.sh/ruff/rules/except-with-empty-tuple/) handle it.
```console
$ cat >sim105_1.py <<'# EOF'
try:
    1 / 0
except ():
    pass
# EOF

$ python sim105_1.py; echo $?
Traceback (most recent call last):
  File "sim105_1.py", line 2, in <module>
    1 / 0
    ~~^~~
ZeroDivisionError: division by zero
1

$ ruff --isolated check sim105_1.py --select SIM105 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim105_1.py
import contextlib
with contextlib.suppress(Exception):
    1 / 0

$ python sim105_1.py; echo $?
0
```

The fix also changes behavior when the `except` clause has no expression. Instead of `contextlib.suppress(Exception)`, the replacement should be `contextlib.suppress(BaseException)`. Alternatively, the rule (or just the fix) could be suppressed, letting [`bare-except` (E722)](https://docs.astral.sh/ruff/rules/bare-except/) handle it.
```console
$ cat >sim105_2.py <<'# EOF'
import sys
try:
    sys.exit(2)
except:
    pass
# EOF

$ python sim105_2.py; echo $?
0

$ ruff --isolated check sim105_2.py --select SIM105 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim105_2.py                                                       
import sys
import contextlib
with contextlib.suppress(Exception):
    sys.exit(2)

$ python sim105_2.py; echo $?
2
```

### Version

ruff 0.11.10 (b35bf8ae0 2025-05-15)

---

_Label `bug` added by @ntBre on 2025-05-20 02:18_

---

_Label `fixes` added by @ntBre on 2025-05-20 02:18_

---

_Label `help wanted` added by @ntBre on 2025-05-20 02:18_

---

_Comment by @VascoSch92 on 2025-05-20 06:13_

I can give a try to that :-) It should be not so hard.

---

_Closed by @ntBre on 2025-06-23 14:01_

---
