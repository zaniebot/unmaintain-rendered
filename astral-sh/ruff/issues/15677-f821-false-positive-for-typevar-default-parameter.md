```yaml
number: 15677
title: "F821 false positive for `TypeVar` `default` parameter unquoted forward reference in type stub (.pyi)"
type: issue
state: closed
author: x11x
labels:
  - bug
assignees: []
created_at: 2025-01-22T22:12:10Z
updated_at: 2025-01-22T23:04:22Z
url: https://github.com/astral-sh/ruff/issues/15677
synced_at: 2026-01-12T15:54:54Z
```

# F821 false positive for `TypeVar` `default` parameter unquoted forward reference in type stub (.pyi)

---

_@x11x_

### Description

Ruff gives [undefined-name (F821)](https://docs.astral.sh/ruff/rules/undefined-name/#undefined-name-f821) error for unquoted forward reference in [`typing.TypeVar`](https://docs.python.org/3/library/typing.html#typing.TypeVar) `default` parameter in .pyi type stub.

`TypeVar` has a new parameter `default` in Python 3.13, which accepts a type.

From what I understand, unquoted forward references should be allowed everywhere **in type stubs**. This has already been fixed for the `bound` parameter, it seems -- F821 is not reported for unquoted forward reference used in `bound` parameter. I think `default` should be treated the same way as `bound`.

Example `.pyi` type stub:
```python
from typing import TypeVar

_P = TypeVar("_P", bound=X, default=X)
                                   #^ F821

# The `bound` parameter does not cause an error, only `default` parameter
_Q = TypeVar("_Q", bound=X)  # (no error)

class X:
    ...
```

---

_Comment by @x11x on 2025-01-22 22:27_

Related: #3011

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-22 22:42_

---

_Label `bug` added by @AlexWaygood on 2025-01-22 22:42_

---

_Comment by @InSyncWithFoo on 2025-01-22 22:43_

@AlexWaygood Oops, sorry, didn't see you there.

---

_Closed by @AlexWaygood on 2025-01-22 23:04_

---
