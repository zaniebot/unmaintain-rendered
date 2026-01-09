---
number: 19326
title: PLR2044 fix ignores a backslash on the preceding line
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-14T11:29:11Z
updated_at: 2025-07-23T15:56:50Z
url: https://github.com/astral-sh/ruff/issues/19326
synced_at: 2026-01-07T13:12:16-06:00
---

# PLR2044 fix ignores a backslash on the preceding line

---

_Issue opened by @dscorbett on 2025-07-14 11:29_

### Summary

The fix for [`empty-comment` (PLR2044)](https://docs.astral.sh/ruff/rules/empty-comment/) deletes the entire physical line if there is nothing in the line but the comment. It should only do so when the physical line is its own logical line. When the previous line ends with a backslash, the fix should delete the comment but not the entire line, or it should delete the entire line plus the preceding backslash. Otherwise, it can change the program’s behavior. [Example](https://play.ruff.rs/42810b3a-eb5d-48a5-96aa-87fefbd89b86):

```console
$ cat >plr2044.py <<'# EOF'
x = 0 \
#
+1
print(x)
# EOF

$ python plr2044.py
0

$ ruff --isolated check plr2044.py --select PLR2044 --fix
Found 1 error (1 fixed, 0 remaining).

$ python plr2044.py
1
```

### Version

ruff 0.12.3 (5bc81f26c 2025-07-11)

---

_Label `bug` added by @MichaReiser on 2025-07-14 12:36_

---

_Label `fixes` added by @MichaReiser on 2025-07-14 12:36_

---

_Label `help wanted` added by @MichaReiser on 2025-07-14 12:36_

---

_Comment by @soundsonacid on 2025-07-14 16:23_

will take this one

---

_Assigned to @soundsonacid by @ntBre on 2025-07-14 18:19_

---

_Referenced in [astral-sh/ruff#19338](../../astral-sh/ruff/pulls/19338.md) on 2025-07-14 19:58_

---

_Referenced in [astral-sh/ruff#19405](../../astral-sh/ruff/pulls/19405.md) on 2025-07-17 16:21_

---

_Closed by @ntBre on 2025-07-23 15:56_

---
