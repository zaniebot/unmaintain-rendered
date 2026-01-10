```yaml
number: 18231
title: FURB129 fix fails when the variable is parenthesized
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-20T19:36:09Z
updated_at: 2025-05-28T21:01:04Z
url: https://github.com/astral-sh/ruff/issues/18231
synced_at: 2026-01-10T11:09:58Z
```

# FURB129 fix fails when the variable is parenthesized

---

_Issue opened by @dscorbett on 2025-05-20 19:36_

### Summary

The fix for [`readlines-in-for` (FURB129)](https://docs.astral.sh/ruff/rules/readlines-in-for/) fails because of a syntax error when the file variable is parenthesized.

```console
$ cat >furb129.py <<'# EOF'
with open("furb129.py") as f:
    for line in (f).readlines():
        pass
# EOF

$ ruff --isolated check furb129.py --select FURB129 --unsafe-fixes --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `furb129.py`, the rule codes FURB129, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

### Version

ruff 0.11.10 (b35bf8ae0 2025-05-15)

---

_Label `bug` added by @ntBre on 2025-05-20 20:30_

---

_Label `fixes` added by @ntBre on 2025-05-20 20:30_

---

_Label `help wanted` added by @ntBre on 2025-05-20 20:30_

---

_Comment by @LaBatata101 on 2025-05-20 22:10_

I'll take this one

---

_Assigned to @LaBatata101 by @ntBre on 2025-05-20 22:37_

---

_Closed by @ntBre on 2025-05-28 21:01_

---

_Closed by @ntBre on 2025-05-28 21:01_

---
