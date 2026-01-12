```yaml
number: 1818
title: "False positive \"invalid base for `typing.NewType`\" with `float`"
type: issue
state: closed
author: LeeeeT
labels:
  - bug
assignees: []
created_at: 2025-12-09T02:00:13Z
updated_at: 2025-12-12T00:43:11Z
url: https://github.com/astral-sh/ty/issues/1818
synced_at: 2026-01-12T15:54:25Z
```

# False positive "invalid base for `typing.NewType`" with `float`

---

_@LeeeeT_

### Summary

```python
from typing import NewType

MyFloat = NewType('MyFloat', float)
```

```
error[invalid-newtype]: invalid base for `typing.NewType`
 --> file.py:3:30
  |
1 | from typing import NewType
2 |
3 | MyFloat = NewType('MyFloat', float)
  |                              ^^^^^ type `int | float`
  |
info: The base of a `NewType` must be a class type or another `NewType`.
info: rule `invalid-newtype` is enabled by default
```

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Label `bug` added by @MichaReiser on 2025-12-09 02:12_

---

_Assigned to @oconnor663 by @carljm on 2025-12-09 02:25_

---

_Added to milestone `Beta` by @carljm on 2025-12-09 02:26_

---

_Comment by @carljm on 2025-12-09 02:29_

@oconnor663 can you look into this? I think `NewType` will need special handling for `float` and `complex`. This should work:

```py
from typing import NewType

MyFloat = NewType('MyFloat', float)

def f(x: MyFloat):
    ...

f(MyFloat(1))
f(MyFloat(1.0))
```

---

_Comment by @MichaReiser on 2025-12-09 07:27_

Do we need the same special casing in other places like base classes etc too?

---

_Comment by @AlexWaygood on 2025-12-09 08:10_

> Do we need the same special casing in other places like base classes etc too?

I don't think so — class bases are parsed as value expressions. The reason why we infer a union here is because the second argument to NewType is parsed as a type expression.

---

_Comment by @MichaReiser on 2025-12-09 08:13_

ah right, I always forget this distinction. 

We have this crime in `ty_ide`. I hope there's a better way to handle `float` in `NewType` inference

https://github.com/astral-sh/ruff/blob/646ad154c37fbec184805b9b36626b7e8782a16c/crates/ty_python_semantic/src/types/ide_support.rs#L154-L178

---

_Closed by @oconnor663 on 2025-12-12 00:43_

---
