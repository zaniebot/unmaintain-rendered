```yaml
number: 1999
title: Wrong type in class field with __future__ annotations, when class has method with the same name
type: issue
state: closed
author: DanilaMihailov
labels: []
assignees: []
created_at: 2025-12-17T11:22:24Z
updated_at: 2025-12-17T11:24:21Z
url: https://github.com/astral-sh/ty/issues/1999
synced_at: 2026-01-12T15:54:26Z
```

# Wrong type in class field with __future__ annotations, when class has method with the same name

---

_@DanilaMihailov_

### Summary

python 3.9.21

If class has method with the same name as builtin type, it gets used for types. 

**Only** when `from __future__ import annotations` used.


```python
from __future__ import annotations


class MyClass:
    field: set[str]

    def set(self): ... # named the same as builtin type
```

Error:
```
error[invalid-type-form]: Invalid subscript of object of type `def set(self) -> Unknown` in type expression
 --> ty_repro.py:5:12
  |
4 | class MyClass:
5 |     field: set[str]
  |            ^^^^^^^^
6 |
7 |     def set(self): ...
  |
info: rule `invalid-type-form` is enabled by default
```

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @AlexWaygood on 2025-12-17 11:24_

Thanks for the report! Please see https://github.com/astral-sh/ty/issues/1747

---

_Closed by @AlexWaygood on 2025-12-17 11:24_

---
