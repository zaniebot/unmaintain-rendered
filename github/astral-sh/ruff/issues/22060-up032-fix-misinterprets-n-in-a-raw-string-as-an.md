---
number: 22060
title: "UP032 fix misinterprets `\\N` in a raw string as an escape sequence"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-12-18T20:45:47Z
updated_at: 2025-12-18T23:07:17Z
url: https://github.com/astral-sh/ruff/issues/22060
synced_at: 2026-01-07T13:12:16-06:00
---

# UP032 fix misinterprets `\N` in a raw string as an escape sequence

---

_Issue opened by @dscorbett on 2025-12-18 20:45_

### Summary

The fix for [`f-string` (UP032)](https://docs.astral.sh/ruff/rules/f-string/) can introduce a runtime error by misinterpreting `\N` in a raw string literal, where it is not an escape sequence. The fix in #21901 should not apply to raw strings. [Example](https://play.ruff.rs/8003ac00-7f7d-458f-8752-3667a768a40d):
```console
$ cat >up032.py <<'# EOF'
print(r"\N{angle}AOB = {angle}°".format(angle=180))
# EOF

$ python up032.py
\N180AOB = 180°

$ ruff --isolated check up032.py --select UP032 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up032.py
print(rf"\N{angle}AOB = {180}°")

$ python up032.py 2>&1 | tail -n 1
NameError: name 'angle' is not defined. Did you mean: 'range'?
```

### Version

ruff 0.14.10 (45bbb4cbf 2025-12-18)

---

_Label `bug` added by @ntBre on 2025-12-18 20:54_

---

_Label `fixes` added by @ntBre on 2025-12-18 20:54_

---

_Label `help wanted` added by @ntBre on 2025-12-18 20:54_

---

_Comment by @denyszhak on 2025-12-18 22:13_

I'd love to grab this one

---

_Assigned to @denyszhak by @ntBre on 2025-12-18 23:07_

---

_Referenced in [astral-sh/ruff#22149](../../astral-sh/ruff/pulls/22149.md) on 2025-12-22 22:59_

---
