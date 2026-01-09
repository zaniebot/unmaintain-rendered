---
number: 19433
title: RUF010 fix should parenthesize generator expressions
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-19T21:31:34Z
updated_at: 2025-07-30T15:02:32Z
url: https://github.com/astral-sh/ruff/issues/19433
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF010 fix should parenthesize generator expressions

---

_Issue opened by @dscorbett on 2025-07-19 21:31_

### Summary

The fix for [`explicit-f-string-type-conversion` (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/) introduces a syntax error when the formatted expression is an unparenthesized generator. [Example](https://play.ruff.rs/94ef25a8-861d-4694-83b5-18a75ff34d8f):
```console
$ cat >ruf010.py <<'# EOF'
f"{ascii(x for x in [])}"
# EOF

$ ruff --isolated check ruf010.py --select RUF010 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Referenced in [astral-sh/ruff#19434](../../astral-sh/ruff/pulls/19434.md) on 2025-07-20 02:17_

---

_Label `bug` added by @ntBre on 2025-07-20 02:28_

---

_Label `fixes` added by @ntBre on 2025-07-20 02:28_

---

_Closed by @ntBre on 2025-07-30 15:02_

---
