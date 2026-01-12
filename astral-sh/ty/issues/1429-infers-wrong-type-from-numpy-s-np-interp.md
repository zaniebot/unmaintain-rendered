```yaml
number: 1429
title: "Infers wrong type from numpy's `np.interp`"
type: issue
state: open
author: cooperoptigrid
labels:
  - generics
assignees: []
created_at: 2025-10-24T04:30:41Z
updated_at: 2025-12-20T09:21:51Z
url: https://github.com/astral-sh/ty/issues/1429
synced_at: 2026-01-12T15:54:25Z
```

# Infers wrong type from numpy's `np.interp`

---

_@cooperoptigrid_

`ty`  incorrectly infer `numpy.interp`’s return type as a scalar `float64` instead of an `NDArray[float64]`

## Example code snippet

```python
from typing import TYPE_CHECKING
import numpy as np

def main() -> None:
    x = np.array([0, 1, 2], dtype=np.int64)  # same behaviour with or without this extra dtype (although mypy gives me "Any" without)
    xp = np.array([0, 2, 4], dtype=np.int64)
    fp = np.array([0, 1, 2], dtype=np.int64)

    interpolated_values = np.interp(x, xp, fp)

    if TYPE_CHECKING:
        reveal_type(interpolated_values)  

    print(interpolated_values)


if __name__ == "__main__":
    main()
```

## ty
```sh
ty check main.py
info[revealed-type]: Revealed type
  --> main.py:12:21
   |
11 |     if TYPE_CHECKING:
12 |         reveal_type(interpolated_values)
   |                     ^^^^^^^^^^^^^^^^^^^ `float64`
13 |
14 |     print(interpolated_values)
   |
```

## mypy
```sh
mypy main.py
main.py:12: note: Revealed type is "numpy.ndarray[builtins.tuple[Any, ...], numpy.dtype[numpy.float64]]"
```

## pyright & basedpyright
```sh
pyright main.py  # or basedpyrightmain.py
...:12:21 - information: Type of "interpolated_values" is "ndarray[tuple[Any, ...], dtype[float64]]"
```

## venv
```
Package               Version
--------------------- --------
basedpyright          1.32.1
mypy                  1.18.2
mypy-extensions       1.1.0
nodeenv               1.9.1
nodejs-wheel-binaries 22.20.0
numpy                 2.3.4
pathspec              0.12.1
pyright               1.1.406
ty                    0.0.1a24
typing-extensions     4.15.0
```

## numpy type hints

From [numpy/lib/_function_base_impl.pyi](https://github.com/numpy/numpy/blob/main/numpy/lib/_function_base_impl.pyi#L384) we see

```python
) -> NDArray[complex128 | float64] | complex128 | float64: ...
```

So rather than choosing the `NDArray` overload, it picks the `float64` for some reason.

I assume similar issues are present with other similarly typed numpy functions.

### Version

0.0.1a24

---

_Comment by @AlexWaygood on 2025-10-24 07:00_

Thanks very much for the thorough report! It looks like numpy uses type aliases for its overload annotations here. We don't support type aliases fully yet, and we end up picking the wrong overload as a result 

---

_Closed by @AlexWaygood on 2025-10-24 07:00_

---

_Reopened by @sharkdp on 2025-11-28 19:41_

---

_Comment by @sharkdp on 2025-11-28 19:42_

We now support generic PEP-613 type aliases, but it looks like we will also need to support generic protocols in our constraint solver.

---

_Label `generics` added by @sharkdp on 2025-11-28 19:42_

---

_Renamed from "Infers wrong type from numpy interp" to "Infers wrong type from numpy's `np.interp`" by @sharkdp on 2025-11-28 19:42_

---

_Added to milestone `Beta` by @carljm on 2025-12-01 23:46_

---

_Assigned to @AlexWaygood by @MichaReiser on 2025-12-05 15:48_

---

_Removed from milestone `Beta` by @carljm on 2025-12-16 21:10_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 21:10_

---

_Comment by @feefladder on 2025-12-19 15:30_

In a similar vein, it also doesn't understand `NDArray` types
```
info[revealed-type]: Revealed type
12 |     if TYPE_CHECKING:
13 |         reveal_type(interpolated_values)
14 |         reveal_type(x)
   |                     ^ `Unknown`
```
when digging through the code a bit, it seemed like it got stuck on `TypeAlias`, even though I have `ty --version 0.0.4`

---

_Comment by @fanzeyi on 2025-12-19 23:07_

I think similar issue is happening with `pd.DataFrame(columns=...)` with `list[str]`. It expects a `SequenceNotStr` and ty seems to not be able to deduce the generic parameter: `Expected SequenceNotStr[Unknown], found list[str]`

https://play.ty.dev/3bdbc25e-91a9-45ad-b88c-0b6143270899



---

_Comment by @carljm on 2025-12-20 00:54_

Yes, the root cause of these issues is #1714, which we are actively working on

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-12-20 09:21_

---
