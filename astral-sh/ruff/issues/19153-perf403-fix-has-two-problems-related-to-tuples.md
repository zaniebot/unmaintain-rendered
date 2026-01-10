```yaml
number: 19153
title: PERF403 fix has two problems related to tuples
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-05T13:57:58Z
updated_at: 2025-09-01T14:51:59Z
url: https://github.com/astral-sh/ruff/issues/19153
synced_at: 2026-01-10T11:09:59Z
```

# PERF403 fix has two problems related to tuples

---

_Issue opened by @dscorbett on 2025-07-05 13:57_

### Summary

The fix for [`manual-dict-comprehension` (PERF403)](https://docs.astral.sh/ruff/rules/manual-dict-comprehension/) has two problems related to tuples.

Given a singleton tuple key, the fix changes behavior. [Example](https://play.ruff.rs/a242190c-ec87-4bfa-8423-fb3f87c4cf0a):
```console
$ cat >perf403_1.py <<'# EOF'
def f():
    v = {}
    for o, (x,) in ["ox"]:
        v[x,] = o
    return v
print(f())
# EOF

$ python perf403_1.py
{('x',): 'o'}

$ ruff --isolated check perf403_1.py --select PERF403 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat perf403_1.py
def f():
    v = {x: o for o, (x,) in ["ox"]}
    return v
print(f())

$ python perf403_1.py
{'x': 'o'}
```

Given an unparenthesized tuple value, the fix introduces a syntax error. [Example](https://play.ruff.rs/54d03b7b-dd7c-4297-bacb-fa735245fd10):
```console
$ cat >perf403_2.py <<'# EOF'
def f():
    v = {}
    for (o, p), x in [("op", "x")]:
        v[x] = o, p
# EOF

$ ruff --isolated check perf403_2.py --select PERF403 --preview --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Label `bug` added by @ntBre on 2025-07-06 02:19_

---

_Label `fixes` added by @ntBre on 2025-07-06 02:19_

---

_Label `help wanted` added by @ntBre on 2025-07-06 02:19_

---

_Comment by @liortct on 2025-07-06 06:14_

Hey,
I’d like to fix this issue and open a PR.


---

_Assigned to @liortct by @ntBre on 2025-07-06 14:31_

---

_Comment by @sudormrfbin on 2025-09-01 10:31_

Can this issue be closed now since #19934 is merged?

---

_Comment by @ntBre on 2025-09-01 14:51_

I think so, thank you!

---

_Closed by @ntBre on 2025-09-01 14:51_

---
