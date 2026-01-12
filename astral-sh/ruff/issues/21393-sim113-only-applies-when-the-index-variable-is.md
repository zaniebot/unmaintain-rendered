```yaml
number: 21393
title: "SIM113 only applies when the index variable is initialized to `0`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-11-12T01:03:17Z
updated_at: 2025-11-12T17:54:40Z
url: https://github.com/astral-sh/ruff/issues/21393
synced_at: 2026-01-12T15:54:57Z
```

# SIM113 only applies when the index variable is initialized to `0`

---

_@dscorbett_

### Summary

[`enumerate-for-loop` (SIM113)](https://docs.astral.sh/ruff/rules/enumerate-for-loop/) applies only when the index variable is initialized to `0`, but `enumerate` takes an optional argument specifying its starting point, so SIM113 should only require that the variable start as a strict instance of `int`. For reference, `unnecessary_cast_to_int::call_applicability` and `ResolvedPythonType::from` include various ways to determine whether an expression is an `int`. [Example](https://play.ruff.rs/35196ea9-0532-4208-9fca-f618c3424080):
```console
$ cat >sim113.py <<'# EOF'
fruits = ["apple", "banana", "cherry"]
i = 1
for fruit in fruits:
    print(f"{i}. {fruit}")
    i += 1
# EOF

$ python sim113.py
1. apple
2. banana
3. cherry

$ ruff --isolated check sim113.py --select SIM113
All checks passed!
```

In the spirit of SIM113, that code should be:
```python
fruits = ["apple", "banana", "cherry"]
for i, fruit in enumerate(fruits, 1):
    print(f"{i}. {fruit}")
```

### Version

ruff 0.14.4 (c7ff9826d 2025-11-06)

---

_Label `rule` added by @ntBre on 2025-11-12 13:50_

---

_Closed by @ntBre on 2025-11-12 17:54_

---
