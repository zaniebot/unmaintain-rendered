---
number: 15620
title: "Detect mix of generic type parameters and `Generic` subclass"
type: issue
state: closed
author: ntBre
labels:
  - rule
  - accepted
assignees: []
created_at: 2025-01-20T15:35:09Z
updated_at: 2025-02-06T18:35:53Z
url: https://github.com/astral-sh/ruff/issues/15620
synced_at: 2026-01-07T13:12:16-06:00
---

# Detect mix of generic type parameters and `Generic` subclass

---

_Issue opened by @ntBre on 2025-01-20 15:35_

```pycon
>>> from typing import TypeVar
>>> T = TypeVar("T")
>>> class Foo[S](Generic[T]): ...
... 
Traceback (most recent call last):
  File "<python-input-13>", line 1, in <module>
    class Foo[S](Generic[T]): ...
  File "<python-input-13>", line 1, in <generic parameters of Foo>
    class Foo[S](Generic[T]): ...
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1279, in _generic_init_subclass
    raise TypeError(
        "Cannot inherit from Generic[...] multiple times.")
TypeError: Cannot inherit from Generic[...] multiple times.
```

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/15565#discussion_r1922343790_
            

---

_Label `rule` added by @ntBre on 2025-01-20 15:35_

---

_Referenced in [astral-sh/ruff#15565](../../astral-sh/ruff/pulls/15565.md) on 2025-01-20 15:36_

---

_Label `accepted` added by @AlexWaygood on 2025-01-21 13:07_

---

_Comment by @InSyncWithFoo on 2025-01-30 19:50_

@ntBre Do you intend to work on this? If not, I can take it.

---

_Comment by @ntBre on 2025-01-30 19:52_

Go for it! Thanks for checking.

---

_Assigned to @InSyncWithFoo by @ntBre on 2025-01-30 19:53_

---

_Referenced in [astral-sh/ruff#15841](../../astral-sh/ruff/pulls/15841.md) on 2025-01-31 00:36_

---

_Closed by @AlexWaygood on 2025-02-06 18:35_

---
