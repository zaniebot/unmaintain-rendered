```yaml
number: 19771
title: "UP032 fix misinterprets `\\N` escape sequence as interpolation"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-08-05T19:51:37Z
updated_at: 2025-12-16T21:33:40Z
url: https://github.com/astral-sh/ruff/issues/19771
synced_at: 2026-01-10T11:09:59Z
```

# UP032 fix misinterprets `\N` escape sequence as interpolation

---

_Issue opened by @dscorbett on 2025-08-05 19:51_

### Summary

The fix for [f-string (UP032)](https://docs.astral.sh/ruff/rules/f-string/) can introduce a syntax error by misinterpreting a `\N` escape sequence. [Example](https://play.ruff.rs/a82697b6-035e-40dd-8098-9aa6f563ed56):
```console
$ cat >up032.py <<'# EOF'
"\N{angle}AOB = {angle}°".format(angle=180)
# EOF

$ ruff --isolated check up032.py --select UP032 --diff 2>&1 | grep error: 
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Label `bug` added by @ntBre on 2025-08-06 00:57_

---

_Label `fixes` added by @ntBre on 2025-08-06 00:57_

---

_Label `help wanted` added by @ntBre on 2025-08-06 00:57_

---

_Comment by @phongddo on 2025-12-08 20:51_

Hey @ntBre 👋 I can take a look at this issue if you're fine with it, maybe continue the work from the linked PR https://github.com/astral-sh/ruff/pull/19774 and include a few additional changes.

---

_Comment by @ntBre on 2025-12-08 22:45_

Go for it! I think it's pretty tricky, so don't feel any pressure to work on or finish it, but I'll assign you for now.

---

_Assigned to @phongddo by @ntBre on 2025-12-08 22:45_

---

_Comment by @phongddo on 2025-12-10 18:41_

Hey @ntBre 👋 I've put up https://github.com/astral-sh/ruff/pull/21901. Please review it when you have a moment. Thank you 🙏 (and sorry, I don’t have permission to assign you as a reviewer on my PR).

---

_Closed by @ntBre on 2025-12-16 21:33_

---
