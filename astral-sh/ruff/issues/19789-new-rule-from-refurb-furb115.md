```yaml
number: 19789
title: "New rule from refurb: `FURB115`"
type: issue
state: open
author: chirizxc
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-06T19:04:21Z
updated_at: 2025-08-07T21:37:18Z
url: https://github.com/astral-sh/ruff/issues/19789
synced_at: 2026-01-10T11:09:59Z
```

# New rule from refurb: `FURB115`

---

_Issue opened by @chirizxc on 2025-08-06 19:04_

<img width="1146" height="457" alt="Image" src="https://github.com/user-attachments/assets/ff42d62c-e192-4fe8-99d2-e8815a5d2d50" />

```python
fruits = ["orange", "apple"]

if len(fruits) == 0:
    print("1")

if not fruits:
    print("1")

```
Use `not <...>` instead of `len(<...>) == 0`

https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb115-no-len-compare

---

_Comment by @MeGaGiGaGon on 2025-08-06 19:26_

There is already a similar rule, [len-test (PLC1802)](https://docs.astral.sh/ruff/rules/len-test/#len-test-plc1802), but since `== 0` is a length test it doesn't error. [Example in playground](https://play.ruff.rs/0ed26f41-c745-4a70-b4e2-3d8e5f71c44f)

---

_Label `rule` added by @ntBre on 2025-08-07 11:54_

---

_Label `needs-decision` added by @ntBre on 2025-08-07 11:54_

---

_Comment by @Avasam on 2025-08-07 21:34_

Note this may cause runtime errors with numpy arrays: `ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()`

Just something to keep in mind.

```py
import numpy as np

arr = np.array([[1,1], [2,2], [3,3], [4,4], [5,5]])

print(arr.size)
print(len(arr))
print(bool(arr))
```

```
10
5

Traceback (most recent call last):
  File "./prog.py", line 7, in <module>
ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all() 
```

---
