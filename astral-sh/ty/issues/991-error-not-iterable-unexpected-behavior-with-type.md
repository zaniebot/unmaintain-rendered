```yaml
number: 991
title: "error[not-iterable] unexpected behavior with type narrowing"
type: issue
state: closed
author: alechouse97
labels: []
assignees: []
created_at: 2025-08-14T17:37:36Z
updated_at: 2025-08-14T17:48:05Z
url: https://github.com/astral-sh/ty/issues/991
synced_at: 2026-01-10T02:06:24Z
```

# error[not-iterable] unexpected behavior with type narrowing

---

_Issue opened by @alechouse97 on 2025-08-14 17:37_

### Summary

Below is a simple code snippet that is attempting to index the list `y` by a selection of types. If the index is a slice type, we directly index the list, else list comprehension is used.

```python
import numpy as np

def func(x: int | list | slice | np.ndarray) -> list:

    y = [1, 2, 3, 4, 5, 6]

    rhs = [x] if isinstance(x, int) else x  # rhs should have type (list | slice | ndarray)

    if isinstance(rhs, slice):
        z = y[rhs]
    else:  # rhs should be of type (list | ndarray)
        z = [y[i] for i in rhs]
    
    return z
```

Running `ty check` results in:

```
error[not-iterable]: Object of type `list[Unknown] | (slice[Any, Any, Any] & ~slice[Any, Any, Any]) | (ndarray[@Todo, Unknown] & ~int)` may not be iterable
  --> tmp.py:12:28
   |
10 |         z = y[rhs]
11 |     else:
12 |         z = [y[i] for i in rhs]
   |                            ^^^
13 |     return z
   |
info: It may not have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default
```

On line 12, `rhs` should either be a list or ndarray, both of which are iterable.

Is this a bug? A similar error is not reproduced when running mypy or pyright on this example.

### Version

ty 0.0.1-alpha.18

---

_Comment by @carljm on 2025-08-14 17:48_

Thanks for the report! Yes, this is a bug; it boils down to https://github.com/astral-sh/ty/issues/456

---

_Closed by @carljm on 2025-08-14 17:48_

---
