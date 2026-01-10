---
number: 281
title: "respect return type of `__new__`"
type: issue
state: open
author: nickdrozd
labels:
  - bug
  - calls
assignees: []
created_at: 2025-05-08T18:04:30Z
updated_at: 2025-12-31T16:39:02Z
url: https://github.com/astral-sh/ty/issues/281
synced_at: 2026-01-10T01:51:14Z
---

# respect return type of `__new__`

---

_Issue opened by @nickdrozd on 2025-05-08 18:04_

### Summary

This code passes Ty without any warnings. But when run it will fail half the time.

```python
from __future__ import annotations

from random import randint
from typing import TYPE_CHECKING

class A:
    def __new__(cls) -> int | A:
        if randint(0, 1):
            return object.__new__(cls)
        else:
            return randint(0, 100)

def f(x: A) -> None:
    assert isinstance(x, A)

a = A()

f(a)
```

`f` expects value of type `A`. But `A.__new__` might return `int`. So constructor of `A` should not be assumed to always return `A`. Ty should flag call `f(a)`.

### Version

ty 0.0.0-alpha.7

---

_Comment by @carljm on 2025-05-08 18:08_

Thanks for the report! Yes, it's a known issue that we don't yet handle `__new__` return type properly, we currently assume it returns an instance of the class. But I don't think we had an issue open for it yet, that I can find, so will keep this one!

---

_Label `bug` added by @carljm on 2025-05-08 18:08_

---

_Renamed from "`__new__` constructor not checked properly" to "respect return type of `__new__`" by @carljm on 2025-05-08 18:08_

---

_Label `calls` added by @AlexWaygood on 2025-05-10 18:03_

---

_Added to milestone `GA` by @carljm on 2025-07-23 23:11_

---

_Comment by @dhruvmanila on 2025-08-29 14:10_

This also affects calls like `slice(a, b, c)` where all parameters are of type `int` as the reveal type is `slice[Any, Any, Any]`. The revealed type of the `__new__` call (which is an overloaded function) is `slice[int, int, int]`.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-31 16:39_

---
