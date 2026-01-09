---
number: 19837
title: FLY002 fix causes syntax error with double quotes in the strings
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-08T20:59:07Z
updated_at: 2025-10-03T13:15:59Z
url: https://github.com/astral-sh/ruff/issues/19837
synced_at: 2026-01-07T13:12:16-06:00
---

# FLY002 fix causes syntax error with double quotes in the strings

---

_Issue opened by @MeGaGiGaGon on 2025-08-08 20:59_

### Summary

When the joined items contain both a variable and a string with a double quote, the fix for [static-join-to-f-string (FLY002)](https://docs.astral.sh/ruff/rules/static-join-to-f-string/#static-join-to-f-string-fly002) will cause a syntax error [playground](https://play.ruff.rs/425c7b15-4eaf-47b8-9f5d-2bf428c665b6):
```powershell
PS ~>echo @'
"".join((foo, '"'))
'@ | uvx ruff check --select FLY002 - --fix --unsafe-fixes 2>&1 | rg "syntax error"
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `bug` added by @ntBre on 2025-08-08 21:32_

---

_Label `fixes` added by @ntBre on 2025-08-08 21:32_

---

_Referenced in [astral-sh/ruff#19887](../../astral-sh/ruff/issues/19887.md) on 2025-08-13 00:57_

---

_Referenced in [astral-sh/ruff#20197](../../astral-sh/ruff/pulls/20197.md) on 2025-09-18 16:59_

---

_Referenced in [astral-sh/ruff#20662](../../astral-sh/ruff/pulls/20662.md) on 2025-10-01 06:21_

---

_Closed by @ntBre on 2025-10-03 13:15_

---
