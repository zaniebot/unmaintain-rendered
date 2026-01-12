```yaml
number: 18507
title: RUF059 fix renames variables without checking existing variable names
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-06T17:40:31Z
updated_at: 2025-06-11T05:58:06Z
url: https://github.com/astral-sh/ruff/issues/18507
synced_at: 2026-01-12T15:54:56Z
```

# RUF059 fix renames variables without checking existing variable names

---

_@dscorbett_

### Summary

The fix for [`unused-unpacked-variable` (RUF059)](https://docs.astral.sh/ruff/rules/unused-unpacked-variable/) can rename a variable to shadow or rebind an existing variable.

```console
$ cat >ruf059.py <<'# EOF'
def f(_x):
    x, = "1"
    print(_x)
f("0")
# EOF

$ python ruf059.py
0

$ ruff --isolated check ruf059.py --select RUF059 --preview --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf059.py
def f(_x):
    _x, = "1"
    print(_x)
f("0")

$ python ruf059.py
1
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-06 17:48_

---

_Label `fixes` added by @ntBre on 2025-06-06 17:48_

---

_Label `help wanted` added by @ntBre on 2025-06-06 17:48_

---

_Closed by @MichaReiser on 2025-06-11 05:58_

---
