```yaml
number: 11887
title: SLOT002 gives a false positive because does take into account multiple inheritance
type: issue
state: closed
author: alessio-locatelli
labels:
  - bug
assignees: []
created_at: 2024-06-16T07:47:20Z
updated_at: 2024-07-26T14:24:42Z
url: https://github.com/astral-sh/ruff/issues/11887
synced_at: 2026-01-10T11:09:53Z
```

# SLOT002 gives a false positive because does take into account multiple inheritance

---

_Issue opened by @alessio-locatelli on 2024-06-16 07:47_

### Search I have tried

`SLOT002` 

There are no duplicate issues.

### Problem

Ruff says to me:
```
SLOT002 Subclasses of `collections.namedtuple()` should define `__slots__`
```

But I inherit from `namedtuple` and `Enum` at the same time.

### Reproducible example

```py
from collections import namedtuple
from enum import Enum


class Foobar(namedtuple("conventional_sign", "name description"), Enum):
    __slots__ = ()
    FOO = 1, "Hello, World!"
    BAR = 2, "Some text here"

    @classmethod
    def item_description(cls, conventional_sign: int) -> str:
        for value in cls.__members__.values():
            if conventional_sign == value[0]:
                return value[1]
        raise ValueError(f"{conventional_sign} not in {cls.__members__.values()}")
```

### Correct (expected) behavior

Look on all parent classes before recommending to add `__slots__`. 

For example, `Enum` has no `__slots__` (https://github.com/python/cpython/issues/90290).


### Versions

- Python 3.12.4
- ruff 0.4.9


---

_Comment by @dhruvmanila on 2024-06-18 06:21_

I think this is a bug. I mainly tested it out by getting the size of the class using the following snippet:
```py
import sys
from collections import namedtuple
from enum import Enum


class NamedTupleNoSlots(namedtuple("Point", ["x", "y"])):
    pass


class NamedTupleWithSlots(namedtuple("Point", ["x", "y"])):
    __slots__ = ()


class NamedTupleAndEnumNoSlots(namedtuple("Point", ["x", "y"]), Enum):
    pass


class NamedTupleAndEnumWithSlots(namedtuple("Point", ["x", "y"]), Enum):
    __slots__ = ()


for cls in (
    NamedTupleNoSlots,
    NamedTupleWithSlots,
    NamedTupleAndEnumNoSlots,
    NamedTupleAndEnumWithSlots,
):
    print(f"{cls.__name__}: {sys.getsizeof(cls)}")
```
Output:
```console
$ python3.13 src/play.py
NamedTupleNoSlots: 1712
NamedTupleWithSlots: 944
NamedTupleAndEnumNoSlots: 1712
NamedTupleAndEnumWithSlots: 1712
```

cc @AlexWaygood I think it should be an "all" behavior instead of "any" behavior for checking against the base class.

---

_Label `bug` added by @dhruvmanila on 2024-06-18 06:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 13:59_

---

_Closed by @charliermarsh on 2024-07-26 14:24_

---

_Closed by @charliermarsh on 2024-07-26 14:24_

---
