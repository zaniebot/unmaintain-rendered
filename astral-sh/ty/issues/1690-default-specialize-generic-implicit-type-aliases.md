```yaml
number: 1690
title: Default-specialize generic implicit type aliases
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - generics
assignees: []
created_at: 2025-11-30T22:49:02Z
updated_at: 2025-12-03T08:10:46Z
url: https://github.com/astral-sh/ty/issues/1690
synced_at: 2026-01-12T15:54:25Z
```

# Default-specialize generic implicit type aliases

---

_@hauntsaninja_

### Summary

```py
import numpy as np
import numpy.typing as npt

def foo(x: npt.NDArray, y: int):
    x / y
```
mypy, pyright, pyrefly, zuban all accept this

```
 error[unsupported-operator]: Operator `/` is unsupported between objects of type `ndarray[Any, dtype[_ScalarType_co]]` and `int`
 --> x.py:5:5
  |
4 | def foo(x: npt.NDArray, y: int):
5 |     x / y
  |     ^^^^^
  |
info: rule `unsupported-operator` is enabled by default
```

### Version

ty a29

---

_Assigned to @sharkdp by @sharkdp on 2025-12-01 08:25_

---

_Label `bug` added by @sharkdp on 2025-12-01 08:25_

---

_Label `generics` added by @sharkdp on 2025-12-01 08:25_

---

_Comment by @sharkdp on 2025-12-01 08:40_

Thank you for reporting this.

The false positive is related to the TODOs for generic type aliases [here](https://github.com/astral-sh/ruff/blob/a6cbc138d273b86a70ed36e13623d079bd368236/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md?plain=1#L515-L554) (typing `x` as `npt.NDArray[Any]` makes the error go away, but should be equivalent to `npt.NDArray`).

To get the correct return type, we will also have to to support generic protocols in our typevar solver, I think.

---

_Added to milestone `Beta` by @carljm on 2025-12-02 02:39_

---

_Renamed from "False positive on numpy code" to "Default-specialize generic implicit type aliases" by @carljm on 2025-12-02 03:01_

---

_Closed by @sharkdp on 2025-12-03 08:10_

---
