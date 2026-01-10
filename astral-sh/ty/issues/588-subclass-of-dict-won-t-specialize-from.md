```yaml
number: 588
title: "subclass of dict won't specialize from constructor call"
type: issue
state: closed
author: carljm
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-05T23:19:26Z
updated_at: 2025-08-01T19:29:20Z
url: https://github.com/astral-sh/ty/issues/588
synced_at: 2026-01-10T02:06:24Z
```

# subclass of dict won't specialize from constructor call

---

_Issue opened by @carljm on 2025-06-05 23:19_

```python
from typing import TypeVar
from typing_extensions import reveal_type

_K = TypeVar("_K")
_V = TypeVar("_V")

class MyDict(dict[_K, _V]):
    pass

a = MyDict(foo=1)
reveal_type(a)
```

which gives

```
error[no-matching-overload] mwe.py:13:5: No overload of bound method `__init__` matches arguments
info[revealed-type] mwe.py:14:13: Revealed type: `MyDict[Unknown, Unknown]`
```

and works with mypy.

 _Originally posted by @MarcinKonowalczyk in [#587](https://github.com/astral-sh/ty/issues/587#issuecomment-2946801679)_

---

_Label `generics` added by @carljm on 2025-06-05 23:19_

---

_Label `bug` added by @carljm on 2025-06-05 23:19_

---

_Assigned to @dcreager by @dcreager on 2025-06-13 19:19_

---

_Comment by @dcreager on 2025-06-23 19:22_

This seems to be the same issue as https://github.com/astral-sh/ruff/pull/18402#issuecomment-2997681949. Something about inheriting the `__init__` method causes our inference to fail, presumably because of the typevars being different in `dict` and `MyDict`.

---

_Comment by @dcreager on 2025-06-23 19:23_

(The linked issue also mentions the inherited `__init__` being a problem — but it's not that it's not _seen_, it's that typevar inference is failing when we try to call it)

---

_Comment by @MatthewMckee4 on 2025-06-24 20:17_

> This seems to be the same issue as [astral-sh/ruff#18402 (comment)](https://github.com/astral-sh/ruff/pull/18402#issuecomment-2997681949). Something about inheriting the `__init__` method causes our inference to fail, presumably because of the typevars being different in `dict` and `MyDict`.

Yeah the generic context of the `MyDict.__init__` contains the dict typevar instead of _V.

Simpler and standalone repro:
```py
from __future__ import annotations

from typing import Generic, TypeVar

T = TypeVar("T")


class A(Generic[T]):
    def __init__(self, x: T):
        self.x = x


V = TypeVar("V")


class B(A[V]): ...


b: B[Unknown] = B(1)
a: A[int] = A(1)

```

---

_Added to milestone `Beta` by @carljm on 2025-07-23 23:03_

---

_Closed by @dcreager on 2025-08-01 19:29_

---
