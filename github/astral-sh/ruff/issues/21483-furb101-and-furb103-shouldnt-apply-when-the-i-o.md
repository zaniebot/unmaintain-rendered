---
number: 21483
title: FURB101 and FURB103 shouldn’t apply when the I/O variable is used later
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-11-16T18:29:44Z
updated_at: 2025-11-17T19:53:30Z
url: https://github.com/astral-sh/ruff/issues/21483
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB101 and FURB103 shouldn’t apply when the I/O variable is used later

---

_Issue opened by @dscorbett on 2025-11-16 18:29_

### Summary

[`read-whole-file` (FURB101)](https://docs.astral.sh/ruff/rules/read-whole-file/) and [`write-whole-file` (FURB103)](https://docs.astral.sh/ruff/rules/write-whole-file/) should be suppressed when the I/O variable is used after the `with` block (unless the variable is unconditionally rebound). There is no straightforward way to get the same behavior by using the suggested `pathlib` methods. [Examples](https://play.ruff.rs/8841cf55-3dfc-4957-9d2c-be3171f3d72d):
```console
$ cat >furb10.py <<'# EOF'
with open("furb10.py", encoding="utf-8") as f1:
    _ = f1.read()
try:
    print(f1.mode)
except NameError as e:
    print(e)

with open("furb10.txt", "w") as f2:
    f2.write("\n")
try:
    print(f2.encoding)
except NameError as e:
    print(e)
# EOF

$ python furb10.py
r
UTF-8

$ ruff --isolated check furb10.py --select FURB101,FURB103 --preview --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat furb10.py
import pathlib
_ = pathlib.Path("furb10.py").read_text(encoding="utf-8")
try:
    print(f1.mode)
except NameError as e:
    print(e)

pathlib.Path("furb10.txt").write_text("\n")
try:
    print(f2.encoding)
except NameError as e:
    print(e)

$ python furb10.py
name 'f1' is not defined
name 'f2' is not defined
```

### Version

ruff 0.14.5 (87dafb878 2025-11-13)

---

_Label `bug` added by @MichaReiser on 2025-11-17 07:27_

---

_Comment by @chirizxc on 2025-11-17 19:31_

i take this

---

_Assigned to @chirizxc by @ntBre on 2025-11-17 19:53_

---
