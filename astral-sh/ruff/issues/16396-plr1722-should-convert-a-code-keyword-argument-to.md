```yaml
number: 16396
title: "PLR1722 should convert a `code` keyword argument to a positional argument"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-02-26T14:19:12Z
updated_at: 2025-03-03T14:20:59Z
url: https://github.com/astral-sh/ruff/issues/16396
synced_at: 2026-01-10T11:09:57Z
```

# PLR1722 should convert a `code` keyword argument to a positional argument

---

_Issue opened by @dscorbett on 2025-02-26 14:19_

### Summary

`exit` and `quit` each have one positional-or-keyword parameter: `code`. `sys.exit` has one positional-only parameter. [`sys-exit-alias` (PLR1722)](https://docs.astral.sh/ruff/rules/sys-exit-alias/) should convert a `code` keyword argument to a positional argument to avoid changing behavior.
```console
$ cat >plr1722.py <<'# EOF'
exit(code=2)
# EOF

$ python plr1722.py; echo $?
2

$ ruff --isolated check --select PLR1722 plr1722.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python plr1722.py; echo $?
Traceback (most recent call last):
  File "plr1722.py", line 2, in <module>
    sys.exit(code=2)
TypeError: sys.exit() takes no keyword arguments
1
```

### Version

ruff 0.9.7 (54fccb3ee 2025-02-20)

---

_Label `bug` added by @AlexWaygood on 2025-02-26 14:31_

---

_Label `fixes` added by @AlexWaygood on 2025-02-26 14:31_

---

_Label `help wanted` added by @AlexWaygood on 2025-02-26 14:31_

---

_Comment by @VascoSch92 on 2025-02-26 14:56_

moreover

```python
code = {"code": 2}
exit(**code)
```

is fixed as

```python
import sys

code = {"code": 2}
sys.exit(**code)
```

which leads to the same error.

---

_Comment by @VascoSch92 on 2025-02-26 16:31_

Actually, I can give a try. It should be fast ;-) 

---

_Assigned to @VascoSch92 by @ntBre on 2025-02-26 16:37_

---

_Closed by @ntBre on 2025-03-03 14:20_

---
