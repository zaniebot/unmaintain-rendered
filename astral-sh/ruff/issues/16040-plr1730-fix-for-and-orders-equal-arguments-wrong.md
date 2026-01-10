---
number: 16040
title: "PLR1730 fix for `<=` and `>=` orders equal arguments wrong"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-02-08T16:39:24Z
updated_at: 2025-02-12T09:27:51Z
url: https://github.com/astral-sh/ruff/issues/16040
synced_at: 2026-01-10T01:22:57Z
---

# PLR1730 fix for `<=` and `>=` orders equal arguments wrong

---

_Issue opened by @dscorbett on 2025-02-08 16:39_

### Description

The fix for [`if-stmt-min-max` (PLR1730)](https://docs.astral.sh/ruff/rules/if-stmt-min-max/) in Ruff 0.9.5 changes the program’s behavior when the operator is `<=` or `>=` and the values are equal. Of the following four examples, the first two were broken by #15930, and the last two were already incorrect.
```console
$ cat >plr1730.py <<'# EOF'
t, c = [], []
if t >= c: t = c
print(t is c)

t, c = [], []
if t <= c: t = c
print(t is c)

t, c = [], []
if c <= t: t = c
print(t is c)

t, c = [], []
if c >= t: t = c
print(t is c)
# EOF

$ python plr1730.py
True
True
True
True

$ ruff --isolated check --preview --select PLR1730 plr1730.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat plr1730.py
t, c = [], []
t = min(t, c)
print(t is c)

t, c = [], []
t = max(t, c)
print(t is c)

t, c = [], []
t = min(t, c)
print(t is c)

t, c = [], []
t = max(t, c)
print(t is c)

$ python plr1730.py
False
False
False
False
```

---

_Label `bug` added by @ntBre on 2025-02-08 16:44_

---

_Label `fixes` added by @ntBre on 2025-02-08 16:44_

---

_Label `help wanted` added by @MichaReiser on 2025-02-08 16:55_

---

_Comment by @VascoSch92 on 2025-02-08 17:07_

Oh sorry... This is my fault 🙈

Can I re-fix it? Now I understand better the problem, and I would like to remediate.


---

_Comment by @AlexWaygood on 2025-02-08 17:45_

@VascoSch92 of course!

---

_Assigned to @VascoSch92 by @AlexWaygood on 2025-02-08 17:45_

---

_Comment by @MichaReiser on 2025-02-08 17:47_

> Oh sorry... This is my fault 🙈

Don't worry. This is really on me. I should have caught that during the review. Thanks for taking another try at it.

---

_Referenced in [astral-sh/ruff#16080](../../astral-sh/ruff/pulls/16080.md) on 2025-02-10 15:07_

---

_Closed by @MichaReiser on 2025-02-12 09:27_

---
