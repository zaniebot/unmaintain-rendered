---
number: 18813
title: "[`Pylint`] [preview] `PLC2801` fix can create a syntax error"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-20T05:49:22Z
updated_at: 2025-06-23T14:15:54Z
url: https://github.com/astral-sh/ruff/issues/18813
synced_at: 2026-01-07T13:12:16-06:00
---

# [`Pylint`] [preview] `PLC2801` fix can create a syntax error

---

_Issue opened by @MeGaGiGaGon on 2025-06-20 05:49_

### Summary

The fix for [unnecessary-dunder-call (PLC2801)](https://docs.astral.sh/ruff/rules/unnecessary-dunder-call/#unnecessary-dunder-call-plc2801) can create a syntax error due to lack of padding if it is directly after a keyword. This has not been brought up yet in either #16053 or #16216. [playground](https://play.ruff.rs/f5c95f14-4962-4759-9158-77f7e70831fa)
```
PS D:\python_projects> Get-Content issue.py
```
```py
three = 1 if 1 else(3.0).__str__()
```
```
PS D:\python_projects> uvx ruff check issue.py --isolated --select PLC --unsafe-fixes --fix --preview
```
```snap

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes PLC2801, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:1:20: PLC2801 Unnecessary dunder call to `__str__`. Use `str()` builtin.
  |
1 | three = 1 if 1 else(3.0).__str__()
  |                    ^^^^^^^^^^^^^^^ PLC2801
  |
  = help: Use `str()` builtin

Found 1 error.
[*] 1 fixable with the --fix option.
```

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Referenced in [astral-sh/ruff#18817](../../astral-sh/ruff/issues/18817.md) on 2025-06-20 06:54_

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:54_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:54_

---

_Label `help wanted` added by @MichaReiser on 2025-06-20 06:54_

---

_Referenced in [astral-sh/ruff#18857](../../astral-sh/ruff/pulls/18857.md) on 2025-06-21 21:22_

---

_Closed by @ntBre on 2025-06-23 14:15_

---
