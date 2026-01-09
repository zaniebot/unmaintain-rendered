---
number: 20919
title: "FURB105 fix should be unsafe when it removes a `sep` with a possibly incorrect type"
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-10-16T12:55:47Z
updated_at: 2025-10-17T20:20:16Z
url: https://github.com/astral-sh/ruff/issues/20919
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB105 fix should be unsafe when it removes a `sep` with a possibly incorrect type

---

_Issue opened by @dscorbett on 2025-10-16 12:55_

### Summary

`print`’s `sep` argument must be `None` or a `str`, even if it is not used, or it raises an error. The fix for [print-empty-string (FURB105)](https://docs.astral.sh/ruff/rules/print-empty-string/) should be marked unsafe when it removes a `sep` whose type it can’t prove is correct, because that can change the program’s behavior. [Example](https://play.ruff.rs/1f713729-0a61-417d-b254-bf03cfd3798c):
```console
$ cat >furb105.py <<'# EOF'
def p(sep):
    print(1, sep=sep)
p(0)
# EOF

$ python furb105.py 2>&1 | tail -n 1
TypeError: sep must be None or a string, not int

$ ruff --isolated check furb105.py --select FURB105 --fix
Found 1 error (1 fixed, 0 remaining).

$ python furb105.py
1
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Label `fixes` added by @ntBre on 2025-10-17 20:20_

---
