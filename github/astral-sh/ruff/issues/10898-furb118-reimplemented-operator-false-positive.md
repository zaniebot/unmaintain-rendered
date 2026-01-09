---
number: 10898
title: "(🐞) `FURB118` (reimplemented-operator) false positive when reference in the item position"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-04-12T06:41:08Z
updated_at: 2024-05-03T20:03:13Z
url: https://github.com/astral-sh/ruff/issues/10898
synced_at: 2026-01-07T13:12:15-06:00
---

# (🐞) `FURB118` (reimplemented-operator) false positive when reference in the item position

---

_Issue opened by @KotlinIsland on 2024-04-12 06:41_

```py
b = 1
def f(a):  # Use `operator.itemgetter(b)` instead of defining a function
    return a[b]
b = 2
f([1, 2, 3])  # 3
```
[playground](https://play.ruff.rs/d230918e-ada7-440b-ad19-dc378656ed86)
but if we follow that direction:
```py
b = 1
f = operator.itemgetter(b)
b = 2
f([1, 2, 3])  # 2
```

# Original issue:
```py
class A:
    def __init__(self):
        self.foo = 1
    
    @property
    def f(self) -> object:  # Use `operator.itemgetter(self.foo)` instead of defining a function
        return self[self.foo]
```
[playground](https://play.ruff.rs/d230918e-ada7-440b-ad19-dc378656ed86)

This function references an input in the value position, making it impossible to rewrite as an attribute
```py
class A:
    def __init__(self):
        self.foo = 1

    f = property(operator.itemgetter(self.foo))
``` 

And if we define it in the `__init__` then it would always use the intial value of `foo`:
```py

class A:
    def __init__(self):
        self.foo = 1
        self.f = property(operator.itemgetter(self.foo))
```

---

_Renamed from "(🐞) `FURB118` (reimplemented-operator) false positive" to "(🐞) `FURB118` (reimplemented-operator) false positive when reference in the item position" by @KotlinIsland on 2024-04-12 06:45_

---

_Label `fixes` added by @MichaReiser on 2024-04-12 07:33_

---

_Comment by @MichaReiser on 2024-04-12 07:35_

Yeah, that's certainly not safe. We should either mark the fix as unsafe or "properly" fix (although that would likely require to replace all call-sites of `f` which I don't think we can do easily)

---

_Label `bug` added by @MichaReiser on 2024-04-12 07:35_

---

_Comment by @KotlinIsland on 2024-04-12 08:04_

Or detect if there is a reference in the item then don't try to fix it?

---

_Comment by @DetachHead on 2024-04-15 02:20_

shouldn't it just not even report the error at all in that case, since it means there's no way to properly convert it to an `itemgetter`

---

_Comment by @KotlinIsland on 2024-04-15 02:24_

Could be a constant

---

_Referenced in [astral-sh/ruff#11045](../../astral-sh/ruff/issues/11045.md) on 2024-04-19 16:01_

---

_Referenced in [astral-sh/ruff#11270](../../astral-sh/ruff/pulls/11270.md) on 2024-05-03 19:55_

---

_Closed by @charliermarsh on 2024-05-03 20:03_

---
