```yaml
number: 2319
title: "[BUG] Incorrect type inference with `dataclass_transform` and generics / decorators"
type: issue
state: closed
author: liblaf
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2026-01-04T07:00:23Z
updated_at: 2026-01-10T13:45:46Z
url: https://github.com/astral-sh/ty/issues/2319
synced_at: 2026-01-12T15:54:26Z
```

# [BUG] Incorrect type inference with `dataclass_transform` and generics / decorators

---

_@liblaf_

### Summary

When using `@dataclass_transform()` in combination with generics and overloads, `ty` appears to infer the wrong type for the result of the dataclass-like decorator. This differs from the behavior of the standard `dataclasses.dataclass` function:

#### Example / Minimal Repro:
```python
import dataclasses
from collections.abc import Callable
from typing import dataclass_transform, overload, reveal_type

@dataclass_transform()
@overload
def func[T: type](cls: T) -> T: ...
@dataclass_transform()
@overload
def func[T: type]() -> Callable[[T], T]: ...
def func[T: type](cls: T | None = None) -> T | Callable[[T], T]:
    raise NotImplementedError

class A: ...

B = dataclasses.dataclass(A)
reveal_type(B)  # `<class 'A'>` as expected

C = func(A)
# actual: `<decorator produced by dataclass-like function>`
# expected: `<class 'A'>`
reveal_type(C)
```

#### Expected Behavior

The result of `func(A)` should be inferred as `<class 'A'>`, matching the behavior of the standard `dataclasses.dataclass` application.

#### Actual Behavior

'ty' reports `<decorator produced by dataclass-like function>` instead of `<class 'A'>` for `reveal_type(C)`.

#### Environment

- ty command: ty check main.py
- playground link: <https://play.ty.dev/9cb27674-5bb8-4cb5-abe4-f49ec8cd2fb2>

---

Let me know if further details or logs are needed!

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-04 22:38_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-04 22:38_

---

_Added to milestone `Stable` by @carljm on 2026-01-04 22:38_

---

_Comment by @carljm on 2026-01-04 22:43_

Yes, this looks like a bug -- thanks for the report. Other type checkers give the expected result here.

It seems like we have something special-cased for dataclass transforms in class-decorator application, since this works as expected (appended to the above):

```py
@func
class D:
    x: int

D(x=1)
```

The overloads don't seem to be necessary to repro: https://play.ty.dev/f337dde7-2cb7-4226-959c-592bb6b9a494

```py
import dataclasses
from collections.abc import Callable
from typing import dataclass_transform, overload, reveal_type

@dataclass_transform()
def func[T: type](cls: T) -> T:
    return cls

class A: ...

B = dataclasses.dataclass(A)
reveal_type(B)  # `<class 'A'>` as expected

C = func(A)
# actual: `<decorator produced by dataclass-like function>`
# expected: `<class 'A'>`
reveal_type(C)
```

Another manifestation of the bug is that this "works" (i.e. applies a dataclass transform to `D`) when it should be an error:

```py
@func(A)
class D:
    x: int

D(x=1)
```

---

_Label `bug` added by @AlexWaygood on 2026-01-04 22:44_

---

_Label `dataclasses` added by @AlexWaygood on 2026-01-04 22:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 23:14_

---

_Closed by @charliermarsh on 2026-01-10 13:45_

---
