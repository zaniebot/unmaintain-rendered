```yaml
number: 20287
title: "`needless-bool` (SIM103) could be safe for expressions guaranteed to return `bool`s"
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-09-07T18:31:20Z
updated_at: 2025-09-10T14:15:24Z
url: https://github.com/astral-sh/ruff/issues/20287
synced_at: 2026-01-10T11:09:59Z
```

# `needless-bool` (SIM103) could be safe for expressions guaranteed to return `bool`s

---

_Issue opened by @dscorbett on 2025-09-07 18:31_

### Summary

The fix for [`needless-bool` (SIM103)](https://docs.astral.sh/ruff/rules/needless-bool/) is marked unsafe, but it could be marked safe when the replacement is a `bool` call, `not` expression, `in` expression, `not in` expression, `is` expression, or `is not` expression, all of which are guaranteed to return a `bool`. [Example](https://play.ruff.rs/c9353445-2d3a-4578-b1c4-00619a2bbf1f):
```console
$ cat >sim103.py <<'# EOF'
def f(x):
    if x:
        return True
    return False
print(f(0), f(1))

def f(x):
    if x:
        return False
    return True
print(f(0), f(1))

def f(x):
    if x in [0]:
        return True
    return False
print(f(0), f(1))

def f(x):
    if x in [0]:
        return False
    return True
print(f(0), f(1))

def f(x):
    if x not in [0]:
        return True
    return False
print(f(0), f(1))

def f(x):
    if x not in [0]:
        return False
    return True
print(f(0), f(1))

def f(x):
    if x is x:
        return True
    return False
print(f(0))

def f(x):
    if x is x:
        return False
    return True
print(f(0))

def f(x):
    if x is not x:
        return True
    return False
print(f(0))

def f(x):
    if x is not x:
        return False
    return True
print(f(0))
# EOF

$ python sim103.py
False True
True False
True False
False True
False True
True False
True
False
False
True

$ ruff --isolated check sim103.py --select SIM103 --unsafe-fixes --fix
Found 10 errors (10 fixed, 0 remaining).

$ python sim103.py
False True
True False
True False
False True
False True
True False
True
False
False
True
```

### Version

ruff 0.12.12 (c6516e9b6 2025-09-04)

---

_Label `fixes` added by @ntBre on 2025-09-08 14:11_

---

_Comment by @TaKO8Ki on 2025-09-10 12:17_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-10 14:15_

---
