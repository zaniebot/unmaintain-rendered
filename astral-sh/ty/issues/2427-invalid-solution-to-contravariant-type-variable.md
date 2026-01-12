```yaml
number: 2427
title: Invalid solution to contravariant type variable with upper bound
type: issue
state: closed
author: ibraheemdev
labels:
  - generics
assignees: []
created_at: 2026-01-09T21:52:26Z
updated_at: 2026-01-12T10:00:37Z
url: https://github.com/astral-sh/ty/issues/2427
synced_at: 2026-01-12T11:01:03Z
```

# Invalid solution to contravariant type variable with upper bound

---

_Issue opened by @ibraheemdev on 2026-01-09 21:52_

For example,

```py
class Contra[T]:
    def append(self, x: T): ...

def f[T: int](x: Contra[T]):
    ...

def _(x: Contra[str]):
    f(x) # Argument to function `f` is incorrect: Argument type `str` does not satisfy upper bound `int` of type variable `T`
```

Solving `T` to `int & str = Never` would avoid the assignability error.

---

_Label `generics` added by @ibraheemdev on 2026-01-09 21:52_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 22:40_

---

_Closed by @dcreager on 2026-01-12 10:00_

---
