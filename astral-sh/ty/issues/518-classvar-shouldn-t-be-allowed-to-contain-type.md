```yaml
number: 518
title: "`ClassVar` shouldn't be allowed to contain type variables"
type: issue
state: open
author: karlicoss
labels:
  - bug
  - generics
  - typing semantics
assignees: []
created_at: 2025-05-26T22:58:50Z
updated_at: 2025-11-13T06:48:58Z
url: https://github.com/astral-sh/ty/issues/518
synced_at: 2026-01-12T15:54:23Z
```

# `ClassVar` shouldn't be allowed to contain type variables

---

_@karlicoss_

### Summary

See 
https://typing.python.org/en/latest/spec/class-compat.html#classvar

```
Note that a ClassVar parameter cannot include any type variables, regardless of the level of nesting: ClassVar[T] and ClassVar[list[set[T]]] are both invalid if T is a type variable.
```


I find their example unnecessarily complicated, here's a simpler reproducer
```
from typing import ClassVar, reveal_type, cast

class Box[T]:
    value: ClassVar[T] = cast(T, None) 

int_box = Box[int]()
reveal_type(int_box.value)

str_box = Box[str]()
reveal_type(str_box.value)
```

Currently this passes, inferring conflicting types:
```
info[revealed-type]: Revealed type
 --> test_classvar.py:7:13
  |
6 | int_box = Box[int]()
7 | reveal_type(int_box.value)
  |             ^^^^^^^^^^^^^ `int`
8 |
9 | str_box = Box[str]()
  |

info[revealed-type]: Revealed type
  --> test_classvar.py:10:13
   |
 9 | str_box = Box[str]()
10 | reveal_type(str_box.value)
   |             ^^^^^^^^^^^^^ `str`
   |

Found 2 diagnostics
```

# Expected result
Should be rejected, e.g. mypy rejects this with `error: ClassVar cannot contain type variables  [misc]`

# Relevant issues
- perhaps somewhat relevant to https://github.com/astral-sh/ty/issues/166



### Version

ty 0.0.1-alpha.7

---

_Label `bug` added by @AlexWaygood on 2025-05-26 23:23_

---

_Label `generics` added by @AlexWaygood on 2025-05-26 23:23_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-26 23:23_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:48_

---
