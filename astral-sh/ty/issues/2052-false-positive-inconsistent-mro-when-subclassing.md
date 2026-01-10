---
number: 2052
title: "False-positive `inconsistent-mro` when subclassing `Generic` and a \"protocol-like ABC\""
type: issue
state: open
author: gschaffner
labels:
  - bug
assignees: []
created_at: 2025-12-18T06:21:46Z
updated_at: 2025-12-23T05:17:35Z
url: https://github.com/astral-sh/ty/issues/2052
synced_at: 2026-01-10T01:52:52Z
---

# False-positive `inconsistent-mro` when subclassing `Generic` and a "protocol-like ABC"

---

_Issue opened by @gschaffner on 2025-12-18 06:21_

### Summary

Reproducer ([playground](https://play.ty.dev/b69404c6-38c0-4969-b4b8-f3755b668c5a)):

```python
from contextlib import AbstractContextManager
from typing import Generic
from typing import TypeVar

T = TypeVar("T")


class A(Generic[T], AbstractContextManager):  # error: inconsistent-mro
    ...


class B(AbstractContextManager, Generic[T]):  # no error
    ...
```

Output:

```console
$ ty check eg.py
error[inconsistent-mro]: Cannot create a consistent method resolution order (MRO) for class `A` with bases list `[<special-form 'typing.Generic[T]'>, <class 'AbstractContextManager'>]`
 --> eg.py:8:1
  |
8 | / class A(Generic[T], AbstractContextManager):  # error: inconsistent-mro
9 | |     ...
  | |_______^
  |
info: rule `inconsistent-mro` is enabled by default

Found 1 diagnostic
```

https://github.com/python/typeshed/pull/13005 is related.

### Version

ty 0.0.3

---

_Comment by @carljm on 2025-12-22 23:30_

Thanks for reporting this!

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:31_

---

_Label `bug` added by @dhruvmanila on 2025-12-23 05:17_

---
