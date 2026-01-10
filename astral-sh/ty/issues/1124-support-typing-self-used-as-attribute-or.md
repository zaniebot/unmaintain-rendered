---
number: 1124
title: "support `typing.Self` used as attribute (or dataclass field) type"
type: issue
state: open
author: JoanPuig
labels:
  - typing semantics
assignees: []
created_at: 2025-09-03T20:12:58Z
updated_at: 2026-01-09T23:21:32Z
url: https://github.com/astral-sh/ty/issues/1124
synced_at: 2026-01-10T01:48:23Z
---

# support `typing.Self` used as attribute (or dataclass field) type

---

_Issue opened by @JoanPuig on 2025-09-03 20:12_

### Summary

The following code:

```py
from dataclasses import dataclass
from typing import Optional, Self


@dataclass(frozen=True)
class MyClass:
    field: Optional[Self] = None


MyClass(MyClass())
```

reports the following, which I believe should not be an error:

```
error[invalid-argument-type]: Argument is incorrect
  --> src\main.py:10:9
   |
10 | MyClass(MyClass())
   |         ^^^^^^^^^ Expected `typing.Self | None`, found `MyClass`
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```



### Version

0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @carljm on 2025-09-03 20:21_

There's also this dataclass-less version, which is a bit different in that it doesn't involve a synthesized `__init__` method, and which we also wrongly error on:

```py
from typing import Optional, Self


class MyClass:
    field: Optional[Self] = None


def _(c: MyClass):
    c.field = c
```

---

_Label `typing semantics` added by @carljm on 2025-09-03 20:21_

---

_Renamed from "Typing.Self" to "support `typing.Self` used as attribute (or dataclass field) type" by @carljm on 2025-09-03 20:21_

---

_Comment by @carljm on 2025-09-03 20:21_

Thanks for the report!

---

_Added to milestone `GA` by @carljm on 2025-10-10 23:17_

---

_Comment by @Wizzerinus on 2025-12-26 09:21_

Another interesting example which does not involve any assignments. I believe it is a special case of this issue, since the diagnostic only occurs if the Self type is used in a field.

```py
from typing import Generic, TypeVar, Self


class Fruit:
    consumer: "Consumer[Self] | None" = None


T = TypeVar("T", bound=Fruit)


class Consumer(Generic[T]): ...
```

Ty output:

```
error[invalid-type-arguments]: Type `<special-form 'typing.Self'>` is not assignable to upper bound `Fruit` of type variable `T@Consumer`
 --> self_bound.py:5:25
  |
4 | class Fruit:
5 |     consumer: "Consumer[Self] | None" = None
  |                         ^^^^
  |
 ::: self_bound.py:8:1
  |
8 | T = TypeVar("T", bound=Fruit)
  | - Type variable defined here
  |
info: rule `invalid-type-arguments` is enabled by default
```

---

_Removed from milestone `Stable` by @carljm on 2025-12-27 00:37_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-27 00:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 20:55_

---

_Unassigned @charliermarsh by @charliermarsh on 2026-01-09 20:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 23:21_

---
