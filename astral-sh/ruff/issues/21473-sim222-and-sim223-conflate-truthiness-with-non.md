```yaml
number: 21473
title: SIM222 and SIM223 conflate truthiness with non-emptiness
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-11-15T16:53:18Z
updated_at: 2025-12-01T21:57:52Z
url: https://github.com/astral-sh/ruff/issues/21473
synced_at: 2026-01-12T15:54:57Z
```

# SIM222 and SIM223 conflate truthiness with non-emptiness

---

_@dscorbett_

### Summary

`Truthiness::from_expr` assumes that all values are iterable and that truthy is the same as non-empty, causing false positives in [`expr-or-true` (SIM222)](https://docs.astral.sh/ruff/rules/expr-or-true/) and [`expr-and-false` (SIM223)](https://docs.astral.sh/ruff/rules/expr-and-false/). [Examples](https://play.ruff.rs/581a78c0-58cb-4662-945b-bacb45bada01):
```console
$ cat >sim22.py <<'# EOF'
print(tuple(t"") or True)
print(tuple(t"") and False)
try:
    print(tuple(0) or True)
except TypeError as e:
    print(e)
try:
    print(tuple(1) and False)
except TypeError as e:
    print(e)
# EOF

$ python3.14 sim22.py
True
()
'int' object is not iterable
'int' object is not iterable

$ ruff --isolated check sim22.py --select SIM222,SIM223 --unsafe-fixes --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat sim22.py
print(tuple(t""))
print(False)
try:
    print(True)
except TypeError as e:
    print(e)
try:
    print(False)
except TypeError as e:
    print(e)

$ python3.14 sim22.py
()
False
True
False
```

### Version

ruff 0.14.5 (87dafb878 2025-11-13)

---

_Label `bug` added by @MichaReiser on 2025-11-17 07:27_

---

_Closed by @ntBre on 2025-12-01 21:57_

---
