```yaml
number: 1107
title: NamedTuple fields should check as actual type, instead of property
type: issue
state: closed
author: y2kbugger
labels: []
assignees: []
created_at: 2025-08-30T05:19:15Z
updated_at: 2025-12-15T08:55:03Z
url: https://github.com/astral-sh/ty/issues/1107
synced_at: 2026-01-12T15:54:24Z
```

# NamedTuple fields should check as actual type, instead of property

---

_@y2kbugger_

### Summary

https://play.ty.dev/8241691f-6963-4d4d-8dac-31876e2bd78e

pyright already has this behavior

Code sample in [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoByAhhAKYAmAKgK4IA2pANFEQM6ul4D68CpAUIIDGdNqygBZOACUwAdwCCACmJkqtBgEoAXPyj7M5bZhQw9BlCVLHWMEIP4ixkmaVEwKsuSqvr6pHXN9JCMTMwMWYykvBQciIigAXhcYpQBGZgAiBTokIVJMzX4QECSUtyIPci905iJE%2BKLBMU4YHkRSJRLmaIqqryKW7l5OkoA6Ih6ZeQVB9lb2viVe9095CamYzSA)

```python
from typing import NamedTuple, assert_type


class MyRowA(NamedTuple):
    id: int
    name: str


class MyRelatedRow(NamedTuple):
    id: int
    a: MyRowA


aa = MyRowA(1, "Alice")
rr = MyRelatedRow(1, a=aa)


assert_type(rr, MyRelatedRow)
assert_type(rr.a, MyRowA)
assert_type(MyRelatedRow.a, MyRowA)
```



### Version

6f2b874d6

---

_Comment by @AlexWaygood on 2025-08-30 15:26_

Thanks for the report! However, ty is correct here, and pyright is incorrect. It's quite easy to demonstrate that accessing the `NamedTuple` field from the class object itself (rather than an instance of the class) does not return an instance of the annotated type. Instead, it returns a property-like descriptor:

```pycon
>>> from typing import NamedTuple, assert_type
... 
... 
... class MyRowA(NamedTuple):
...     id: int
...     name: str
... 
... 
... class MyRelatedRow(NamedTuple):
...     id: int
...     a: MyRowA
... 
... 
... aa = MyRowA(1, "Alice")
... rr = MyRelatedRow(1, a=aa)
... 
>>> MyRelatedRow.a
_tuplegetter(1, 'Alias for field number 1')
```

To be fully accurate, perhaps ty should infer the type of `MyRelatedRow.a` as `_tuplegetter` when it's accessed from the class object. But I think `property` is probably close enough here; `_tuplegetter` acts like a read-only property in almost every way.

---

_Closed by @AlexWaygood on 2025-08-30 15:26_

---

_Comment by @y2kbugger on 2025-08-30 18:15_

Hmm, I guess I assumed it was a purposeful/useful lie, and so I ~~ab~~used it. But now I wonder if they it is just over looked.


---

_Comment by @y2kbugger on 2025-12-15 05:30_

Dataclasses statically resolve fields on the Class to match the type of the instance field, however accessing that field at runtime is an attribute error. 

edit: updated
https://play.ty.dev/12f60d0d-9075-439c-a403-cb26684fb670

---

_Comment by @AlexWaygood on 2025-12-15 08:55_

> Dataclasses statically resolve fields on the Class to match the type of the instance field, however accessing that field at runtime is an attribute error.
> 
> edit: updated https://play.ty.dev/12f60d0d-9075-439c-a403-cb26684fb670

https://github.com/astral-sh/ty/issues/1522 is the issue for discussing whether that behaviour is desirable or not 

---
