---
number: 18747
title: "FURB163 fix should parenthesize `yield`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-18T12:16:46Z
updated_at: 2025-06-23T08:13:04Z
url: https://github.com/astral-sh/ruff/issues/18747
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB163 fix should parenthesize `yield`

---

_Issue opened by @dscorbett on 2025-06-18 12:16_

### Summary

The fix for [`redundant-log-base` (FURB163)](https://docs.astral.sh/ruff/rules/redundant-log-base/#redundant-log-base-furb163) should parenthesize `yield` expressions to avoid syntax errors.
```console
$ cat >furb163.py <<'# EOF'
import math
def log():
    yield math.log((yield), math.e)
# EOF

$ ruff --isolated check furb163.py --select FURB163 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `bug` added by @MichaReiser on 2025-06-18 14:07_

---

_Label `help wanted` added by @MichaReiser on 2025-06-18 14:07_

---

_Comment by @chirizxc on 2025-06-18 14:20_

`yield from` also

---

_Referenced in [astral-sh/ruff#18756](../../astral-sh/ruff/pulls/18756.md) on 2025-06-18 14:46_

---

_Closed by @MichaReiser on 2025-06-23 08:13_

---
