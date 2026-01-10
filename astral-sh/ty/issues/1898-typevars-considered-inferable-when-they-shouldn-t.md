```yaml
number: 1898
title: "typevars considered inferable when they shouldn't be"
type: issue
state: open
author: carljm
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-15T18:35:52Z
updated_at: 2025-12-15T21:45:48Z
url: https://github.com/astral-sh/ty/issues/1898
synced_at: 2026-01-10T01:55:00Z
```

# typevars considered inferable when they shouldn't be

---

_Issue opened by @carljm on 2025-12-15 18:35_

```py
class Container[T]:
    def method(self, x: T) -> T:
        return x

    def try_assign[U](self, x: U) -> U:
        result = self.method(x)  # false negative here
        reveal_type(result)  # revealed: `T@Container`
        return result  # we correctly emit a diagnostic here
```

https://play.ty.dev/8b3bb7c8-d7a7-447a-9318-7ff50e896f90

I think the `self.method(x)` call should be a type error, but currently is not. `U` and `T` are independent type variables, and neither of them is inferable in the context of this call. `T` is already bound because we are in a method of `Container`; `U` is bound because we are inside `try_assign` method.

---

_Added to milestone `Stable` by @carljm on 2025-12-15 18:35_

---

_Label `bug` added by @carljm on 2025-12-15 18:35_

---

_Label `generics` added by @carljm on 2025-12-15 18:35_

---
