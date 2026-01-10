```yaml
number: 468
title: Implement argument type expansion for overload call evaluation
type: issue
state: closed
author: AA-Turner
labels:
  - bug
  - calls
  - overloads
assignees: []
created_at: 2025-05-20T22:25:58Z
updated_at: 2025-06-04T02:12:02Z
url: https://github.com/astral-sh/ty/issues/468
synced_at: 2026-01-10T02:34:10Z
```

# Implement argument type expansion for overload call evaluation

---

_Issue opened by @AA-Turner on 2025-05-20 22:25_

### Summary

I don't believe this is a duplicate, apologies if otherwise.

```python
from typing import Any
class Spam:
    def __getitem__(self, item: int | slice) -> Any:
        return (1, 'spam', None)[item]
```

`ty` reports: `` `Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` is not callable on object of type `tuple[Literal[1], Literal["spam"], None]` (call-non-callable) [Ln 4, Col 16]` ``

A

https://play.ty.dev/13c25386-6806-4686-880c-7891a3fb9044

### Version

play.ty.dev reports 0.0.0, but locally `ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)`

---

_Label `bug` added by @sharkdp on 2025-05-21 07:06_

---

_Label `calls` added by @sharkdp on 2025-05-21 07:06_

---

_Comment by @sharkdp on 2025-05-21 07:07_

Thank you for reporting this!

It looks like we do not properly handle unions in overload resolution. Here's a self-contained example:
```py
from typing import overload

class A: ...
class B: ...

@overload
def f(x: A) -> int: return 1
@overload
def f(x: B) -> str: return "a"
def f(x: A | B) -> int | str:
    raise NotImplementedError

def _(x: A | B):
    reveal_type(f(x))  # should be int | str, ty: no-matching-overload
```
(https://play.ty.dev/359d3749-dab4-4857-9bb6-b127aaae0910)

---

_Renamed from "``__getitem__`` tuple error `call-non-callable`" to "Incorrect overload resolution for union types" by @sharkdp on 2025-05-21 07:08_

---

_Comment by @sharkdp on 2025-05-21 07:18_

Handling this properly will require "forking" the argument-type-checking algorithm in overload resolution for every possible combination of union elements. For a call like `f(x, y)` with `x: X1 | X2` and `y: Y1 | Y2`, we would need to check all combinations `(X1, Y1); (X1, Y2); (X2, Y1); (X2, Y2)`. If one of these combinations results in a call error, the entire call should fail. Otherwise, the resulting type will be a union of all return types. The important part is that each of these combinations can pick a different overload.

---

_Comment by @MichaReiser on 2025-05-21 07:26_

CC: @dhruvmanila 

---

_Label `overloads` added by @MichaReiser on 2025-05-21 07:26_

---

_Comment by @dhruvmanila on 2025-05-21 13:59_

Yes, this is exactly the thing that I'm currently working on :)

I'll convert this issue to be part of #104.

---

_Renamed from "Incorrect overload resolution for union types" to "Implement argument type expansion for overload call evaluation" by @dhruvmanila on 2025-05-21 14:00_

---

_Added to milestone `Beta` by @dhruvmanila on 2025-05-21 14:01_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-21 15:28_

---

_Closed by @dhruvmanila on 2025-06-04 02:12_

---

_Closed by @dhruvmanila on 2025-06-04 02:12_

---
