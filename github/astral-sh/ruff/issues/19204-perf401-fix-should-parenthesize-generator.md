---
number: 19204
title: PERF401 fix should parenthesize generator expressions
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-08T12:26:35Z
updated_at: 2025-07-23T16:08:16Z
url: https://github.com/astral-sh/ruff/issues/19204
synced_at: 2026-01-07T13:12:16-06:00
---

# PERF401 fix should parenthesize generator expressions

---

_Issue opened by @dscorbett on 2025-07-08 12:26_

### Summary

The fix for [`manual-list-comprehension` (PERF401)](https://docs.astral.sh/ruff/rules/manual-list-comprehension/) changes the program’s behavior when the argument to `append` is an unparenthesized generator expression. It should parenthesize the argument. [Example](https://play.ruff.rs/de4d5dd3-fe00-4d01-977b-87431138e0d1):
```console
$ cat >perf401.py <<'# EOF'
i = "xyz"
result = []
for i in range(3):
   result.append(x for x in [i])
print(list(map(list, result)))
# EOF

$ python perf401.py
[[0], [1], [2]]

$ ruff --isolated check perf401.py --select PERF401 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat perf401.py
i = "xyz"
result = [x for x in [i] for i in range(3)]
print(list(map(list, result)))

$ python perf401.py
[['x', 'y', 'z'], ['x', 'y', 'z'], ['x', 'y', 'z']]
```

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Label `bug` added by @ntBre on 2025-07-08 12:45_

---

_Label `fixes` added by @ntBre on 2025-07-08 12:45_

---

_Label `help wanted` added by @ntBre on 2025-07-08 12:45_

---

_Comment by @CodeMan62 on 2025-07-08 15:36_

i am taking this will fix it 

---

_Assigned to @CodeMan62 by @ntBre on 2025-07-08 16:41_

---

_Referenced in [astral-sh/ruff#19324](../../astral-sh/ruff/pulls/19324.md) on 2025-07-14 09:32_

---

_Referenced in [astral-sh/ruff#19325](../../astral-sh/ruff/pulls/19325.md) on 2025-07-14 09:57_

---

_Closed by @ntBre on 2025-07-23 16:08_

---
