```yaml
number: 1503
title: Method call on constrained typevar emits invalid-argument-type
type: issue
state: open
author: sharkdp
labels:
  - bug
  - generics
  - attribute access
assignees: []
created_at: 2025-11-08T10:14:55Z
updated_at: 2025-12-26T09:11:31Z
url: https://github.com/astral-sh/ty/issues/1503
synced_at: 2026-01-10T01:56:40Z
```

# Method call on constrained typevar emits invalid-argument-type

---

_Issue opened by @sharkdp on 2025-11-08 10:14_

Consider the following example. ty currently emits two errors for the `t.method()` call because `t.method` creates a union of bound method objects where `C.method` and `D.method` are bound to `T@f`, respectively. Instead, attribute access should properly distribute across the union `C | D` and yield a union of bound method objects where `C.method` is bound to an instance of `C`, and `D.method` is bound to an instance of `D`:

```py
class C:
    def method(self) -> int:
        return 0

class D:
    def method(self) -> str:
        return ""

def f[T: (C, D)](t: T):
    # Argument to bound method `method` is incorrect: Expected `C`, found `T@f` (invalid-argument-type)
    # Argument to bound method `method` is incorrect: Expected `D`, found `T@f` (invalid-argument-type)
    reveal_type(t.method())
```
https://play.ty.dev/c102d2bd-de4d-497c-9144-9f45def4196a


Related: https://github.com/astral-sh/ty/issues/138

---

_Label `bug` added by @sharkdp on 2025-11-08 10:14_

---

_Label `generics` added by @sharkdp on 2025-11-08 10:14_

---

_Label `attribute access` added by @sharkdp on 2025-11-08 10:14_

---

_Added to milestone `GA` by @carljm on 2025-11-10 21:04_

---

_Comment by @Wizzerinus on 2025-12-26 09:11_

Another example which seems related to this one:

```py
from typing import TypeVar

Num = TypeVar("Num", int, float)

def add(x: Num, y: Num):
    return x + y  # ty emits an error here
```

---
