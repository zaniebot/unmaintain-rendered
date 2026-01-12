```yaml
number: 853
title: "@dataclass + Subscripted Generics error"
type: issue
state: closed
author: elbaro
labels:
  - bug
  - generics
  - dataclasses
assignees: []
created_at: 2025-07-18T13:57:07Z
updated_at: 2025-07-21T18:51:59Z
url: https://github.com/astral-sh/ty/issues/853
synced_at: 2026-01-12T15:54:24Z
```

# @dataclass + Subscripted Generics error

---

_@elbaro_

### Summary

```py
from dataclasses import dataclass


@dataclass
class Wrap[T]:
    inner: T


class Object: ...


@dataclass
class ObjectWrap(Wrap[Object]): ...


w = ObjectWrap(inner=Object())
print(w)
```

```
  --> tt.py:16:16
   |
16 | w = ObjectWrap(inner=Object())
   |                ^^^^^^^^^^^^^^ Expected `T`, found `Object`
17 | print(w)
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

If you comment `@dataclass` on the outer class, it passes:
```py
# @dataclass
class ObjectWrap(Wrap[Object]): ...
```

### Version

ty 0.0.1-alpha.15

---

_Label `generics` added by @AlexWaygood on 2025-07-18 14:15_

---

_Label `dataclasses` added by @AlexWaygood on 2025-07-18 14:15_

---

_Comment by @carljm on 2025-07-18 18:17_

This [desugared version](https://play.ty.dev/89725d2b-221b-4e10-92ad-dbab2a1463dd) doesn't show the error, so it seems this is dataclasses-specific:

```py
class Wrap[T]:
    inner: T

    def __init__(self, inner: T) -> None:
        self.inner = inner

class Object: ...

class ObjectWrap(Wrap[Object]): ...

w = ObjectWrap(inner=Object())
print(w)
```

---

_Comment by @carljm on 2025-07-18 18:19_

I wonder if `apply_type_mapping` isn't descending into `Callable` type attributes (so we fail to properly specialize the synthetic inherited `__init__` method)

---

_Assigned to @sharkdp by @sharkdp on 2025-07-21 18:24_

---

_Label `bug` added by @sharkdp on 2025-07-21 18:24_

---

_Comment by @sharkdp on 2025-07-21 18:33_

> I wonder if `apply_type_mapping` isn't descending into `Callable` type attributes (so we fail to properly specialize the synthetic inherited `__init__` method)

It was a slightly different bug where the specialization was not passed down to generic dataclass *base classes* (generic dataclasses were already working correctly). I pushed a PR with a fix here: https://github.com/astral-sh/ruff/pull/19472

---

_Closed by @sharkdp on 2025-07-21 18:51_

---
