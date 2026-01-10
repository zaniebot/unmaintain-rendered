```yaml
number: 1995
title: "Invalid assignability for a generic type using `ParamSpec`"
type: issue
state: open
author: tudoroancea
labels:
  - bug
assignees: []
created_at: 2025-12-17T10:09:34Z
updated_at: 2025-12-23T05:26:22Z
url: https://github.com/astral-sh/ty/issues/1995
synced_at: 2026-01-10T01:56:41Z
```

# Invalid assignability for a generic type using `ParamSpec`

---

_Issue opened by @tudoroancea on 2025-12-17 10:09_

### Summary

Hello there,

I have noticed the following weird behavior in `assert_type` when instantiating a generic class using a `ParamSpec` type variable: the type of the resulting instance of the class is correctly inferred, but the assertion ignores it. 

Consider the minimal example below:
```python
from typing import Callable, assert_type, reveal_type


class C[T, **P]:
  def __init__(self, fn: Callable[P, T]):
    self.fn = fn


def f0(x: int) -> int:
  return x


# correctly passes
assert_type((x := C(f0)), C[int, [int]])
reveal_type(x)

# correctly fails
assert_type((x := C(f0)), C[str, [int]])
reveal_type(x)

# passing but shouldn't
assert_type((x := C(f0)), C[int, [str]])
reveal_type(x)
```
which gives the following output when running `ty check assert_type_ignores_paramspec.py` with a Python 3.13 
```
info[revealed-type]: Revealed type
  --> assert_type_ignores_paramspec.py:15:13
   |
13 | # correctly passes
14 | assert_type((x := C(f0)), C[int, [int]])
15 | reveal_type(x)
   |             ^ `C[int, (x: int)]`
16 |
17 | # correctly fails
   |

error[type-assertion-failure]: Argument does not have asserted type `C[str, (int, /)]`
  --> assert_type_ignores_paramspec.py:18:1
   |
17 | # correctly fails
18 | assert_type((x := C(f0)), C[str, [int]])
   | ^^^^^^^^^^^^^----------^^^^^^^^^^^^^^^^^
   |              |
   |              Inferred type is `C[int, (x: int)]`
19 | reveal_type(x)
   |
info: `C[str, (int, /)]` and `C[int, (x: int)]` are not equivalent types
info: rule `type-assertion-failure` is enabled by default

info[revealed-type]: Revealed type
  --> assert_type_ignores_paramspec.py:19:13
   |
17 | # correctly fails
18 | assert_type((x := C(f0)), C[str, [int]])
19 | reveal_type(x)
   |             ^ `C[int, (x: int)]`
   |

info[revealed-type]: Revealed type
  --> assert_type_ignores_paramspec.py:24:13
   |
22 | # passing but shouldn't
23 | assert_type((x := C(f0)), C[int, [str]])
24 | reveal_type(x)
   |             ^ `C[int, (x: int)]`
   |

Found 4 diagnostics
```

You can see that the type of each instance of `C` is correctly inferred (as shown with the `reveal_type`) but only changing the output type makes the assertion fail, not the input paramspec type.

Thank you in advance for your help.

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `bug` added by @AlexWaygood on 2025-12-17 15:19_

---

_Renamed from "assert_type ignores the inferred type of generic using ParamSpec" to "wrong assignability of generic using ParamSpec" by @carljm on 2025-12-22 23:01_

---

_Comment by @carljm on 2025-12-22 23:02_

Thanks for the report! This doesn't seem specific to `assert_type`, there's something wrong with our determination of assignability for a type that's generic over a ParamSpec: https://play.ty.dev/50ed731b-8458-4651-99e7-90e3c462a33f

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:02_

---

_Comment by @dhruvmanila on 2025-12-23 05:24_

> ```py
> # correctly passes
> assert_type((x := C(f0)), C[int, [int]])
> reveal_type(x)
> ```

Even this should fail because the parameter in `f0` is positional or keyword while the one in `C[int, [int]]` is positional-only parameter and `assert_type` performs equivalence check.

---

_Renamed from "wrong assignability of generic using ParamSpec" to "Invalid assignability for a generic type using `ParamSpec`" by @dhruvmanila on 2025-12-23 05:26_

---
