```yaml
number: 21017
title: "UP032 fix doesn’t account for underscores in decimal `int` literals"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-10-21T12:56:46Z
updated_at: 2025-10-22T22:11:52Z
url: https://github.com/astral-sh/ruff/issues/21017
synced_at: 2026-01-10T11:10:00Z
```

# UP032 fix doesn’t account for underscores in decimal `int` literals

---

_Issue opened by @dscorbett on 2025-10-21 12:56_

### Summary

The fix for [`f-string` (UP032)](https://docs.astral.sh/ruff/rules/f-string/) parenthesizes decimal `int` literals when necessary to avoid syntax errors, except for literals containing underscores. [Example](https://play.ruff.rs/feddc757-b6fa-41d3-a78e-8c81b31b6044):
```console
$ cat >up032.py <<'# EOF'
print("{.real}".format(1_2))
# EOF

$ python up032.py
12

$ ruff --isolated check up032.py --select UP032 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.14.1 (2bffef596 2025-10-16)

---

_Comment by @ntBre on 2025-10-21 13:19_

Looks like we'll need to refine this check:

https://github.com/astral-sh/ruff/blob/d109f483ab270105474345ec35c9a3c782b01819/crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs#L164

---

_Label `bug` added by @ntBre on 2025-10-21 13:19_

---

_Label `fixes` added by @ntBre on 2025-10-21 13:19_

---

_Label `help wanted` added by @ntBre on 2025-10-21 13:19_

---

_Comment by @TaKO8Ki on 2025-10-21 15:06_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-10-21 16:23_

---

_Closed by @ntBre on 2025-10-22 22:11_

---
