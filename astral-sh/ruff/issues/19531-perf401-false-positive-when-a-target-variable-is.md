```yaml
number: 19531
title: PERF401 false positive when a target variable is global or nonlocal
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-24T14:19:32Z
updated_at: 2025-07-28T21:03:24Z
url: https://github.com/astral-sh/ruff/issues/19531
synced_at: 2026-01-12T15:54:56Z
```

# PERF401 false positive when a target variable is global or nonlocal

---

_@dscorbett_

### Summary

The fix for [`manual-list-comprehension` (PERF401)](https://docs.astral.sh/ruff/rules/manual-list-comprehension/) is incorrect when the `for` loop’s target list includes a global or nonlocal variable. Rewriting the loop with a comprehension means the variable isn’t updated. This is similar to #16445. [Example](https://play.ruff.rs/33af9aba-7285-4899-9777-bc6b14f6c911):
```console
$ cat >perf401.py <<'# EOF'
INDEX = None
def f():
    global INDEX
    result = []
    for INDEX in range(3):
        result.append(+INDEX)
f()
print(INDEX)
# EOF

$ python perf401.py
2

$ ruff --isolated check perf401.py --select PERF401 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat perf401.py
INDEX = None
def f():
    global INDEX
    result = [+INDEX for INDEX in range(3)]
f()
print(INDEX)

$ python perf401.py
None
```

### Version

ruff 0.12.5 (d13228ab8 2025-07-24)

---

_Label `bug` added by @ntBre on 2025-07-24 14:48_

---

_Label `fixes` added by @ntBre on 2025-07-24 14:48_

---

_Comment by @IDrokin117 on 2025-07-24 16:19_

@ntBre I am going to work on this

---

_Assigned to @IDrokin117 by @ntBre on 2025-07-24 16:54_

---

_Closed by @ntBre on 2025-07-28 21:03_

---
