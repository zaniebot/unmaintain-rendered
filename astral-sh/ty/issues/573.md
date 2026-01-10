```yaml
number: 573
title: "`np.isclose` return value incorrectly inferred as `bool[bool]`"
type: issue
state: closed
author: my1e5
labels: []
assignees: []
created_at: 2025-06-03T09:09:01Z
updated_at: 2025-11-28T19:38:26Z
url: https://github.com/astral-sh/ty/issues/573
synced_at: 2026-01-10T01:58:59Z
```

# `np.isclose` return value incorrectly inferred as `bool[bool]`

---

_Issue opened by @my1e5 on 2025-06-03 09:09_

### Summary

When using [`np.isclose`](https://numpy.org/doc/stable/reference/generated/numpy.isclose.html), ty infers the result as `bool[bool]`, causing it to report that the result is not iterable.

## MRE

```py
from typing import reveal_type

import numpy as np
import numpy.typing as npt

x: npt.NDArray[np.bool_] = np.isclose([1e10, 1e-7], [1.00001e10, 1e-8])

reveal_type(x)

for val in x:
    print(val)
```

## ty

```
$ uvx --with numpy==2.2.6 ty check dev/ty.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
info[revealed-type]: Revealed type
  --> dev\ty.py:8:13
   |
 6 | x: npt.NDArray[np.bool_] = np.isclose([1e10, 1e-7], [1.00001e10, 1e-8])
 7 |
 8 | reveal_type(x)
   |             ^ `bool[bool]`
 9 |
10 | for val in x:
   |

error[not-iterable]: Object of type `bool[bool]` is not iterable
  --> dev\ty.py:10:12
   |
 8 | reveal_type(x)
 9 |
10 | for val in x:
   |            ^
11 |     print(val)
   |
info: It doesn't have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default

Found 2 diagnostics
```

## mypy

```
$ uvx --with numpy==2.2.6 mypy  dev/ty.py 
dev\ty.py:8: note: Revealed type is "numpy.ndarray[builtins.tuple[builtins.int, ...], numpy.dtype[numpy.bool[builtins.bool]]]"
Success: no issues found in 1 source file
```

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @AlexWaygood on 2025-06-03 10:26_

Thanks for the report!

Numpy's stubs for the `isclose()` function are here: https://github.com/numpy/numpy/blob/4c8fd85fbbd035d646bb3c54c47a16b31c1b3ce8/numpy/_core/numeric.pyi#L844-L859. We can see that the type checker is only meant to pick the first overload if the passed-in arguments match the type `_ScalarLike_co`. Otherwise, the type checker is meant to pick the second overload -- if we picked the second overload here, we would infer the correct type.

`_ScalarLike_co`, despite its `*_co` name that would imply it's a covariant TypeVar, is a type alias that uses `typing.TypeAlias`: https://github.com/numpy/numpy/blob/4c8fd85fbbd035d646bb3c54c47a16b31c1b3ce8/numpy/_typing/_scalars.py#L20. We currently do not support type aliases that use `typing.TypeAlias`, which is why we think that a type of `list[float]` is assignable to `_ScalarLike_co` even though it is not.

TL;DR: this will be fixed by #544

---

_Closed by @AlexWaygood on 2025-06-03 10:26_

---

_Closed by @sharkdp on 2025-11-28 19:38_

---
