```yaml
number: 2479
title: "`no-matching-overload` when using `cv2` and `numpy`"
type: issue
state: open
author: Carrot-shreds
labels:
  - needs-info
assignees: []
created_at: 2026-01-13T10:56:34Z
updated_at: 2026-01-13T20:29:42Z
url: https://github.com/astral-sh/ty/issues/2479
synced_at: 2026-01-13T21:36:12Z
```

# `no-matching-overload` when using `cv2` and `numpy`

---

_@Carrot-shreds_

### Summary

Sample code
```py
import cv2
import numpy as np

image = cv2.imread("image.png")
if image is not None:
    np.average(image)
```
It works good and no typing error with pyright

But `ty check` got

```
error[no-matching-overload]: No overload of function `average` matches arguments
 --> src\Model\Data\test.py:6:5
  |
4 | image = cv2.imread("image.png") 
5 | if image is not None:
6 |     np.average(image)
  |     ^^^^^^^^^^^^^^^^^
  |
info: First overload defined here
   --> .venv\Lib\site-packages\numpy\lib\_function_base_impl.pyi:145:5
    |
144 |   @overload
145 |   def average(
    |  _____^
146 | |     a: _ArrayLikeFloat_co,
147 | |     axis: None = None,
148 | |     weights: _ArrayLikeFloat_co | None = None,
149 | |     returned: L[False] = False,
150 | |     *,
151 | |     keepdims: L[False] | _NoValueType = ...,
152 | | ) -> floating: ...
    | |_____________^
153 |   @overload
154 |   def average(
    |
info: Possible overloads for function `average`:
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]]] | int | float | _NestedSequence[int | float], axis: None = None, weights: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]]] | int | ... omitted 3 union elements = None, returned: Literal[False] = False, *, keepdims: Literal[False] | _NoValueType = ...) -> floating[Any]
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]]] | int | float | _NestedSequence[int | float], axis: None = None, weights: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]]]] | int | ... omitted 3 union elements = None, *, returned: Literal[True], keepdims: Literal[False] | _NoValueType = ...) -> tuple[floating[Any], floating[Any]]
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 3 union elements, axis: None = None, weights: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 4 union elements = None, returned: Literal[False] = False, *, keepdims: Literal[False] | _NoValueType = ...) -> complexfloating[Any, Any]
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 3 union elements, axis: None = None, weights: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 4 union elements = None, *, returned: Literal[True], keepdims: Literal[False] | _NoValueType = ...) -> tuple[complexfloating[Any, Any], complexfloating[Any, Any]]
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 5 union elements, axis: SupportsIndex | Sequence[SupportsIndex] | None = None, weights: object = None, *, returned: Literal[True], keepdims: bool | bool[bool] | _NoValueType = ...) -> tuple[Any, Any]
info:   (a: _SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]] | _NestedSequence[_SupportsArray[dtype[bool[bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]] | int | ... omitted 5 union elements, axis: SupportsIndex | Sequence[SupportsIndex] | None = None, weights: object = None, returned: bool | bool[bool] = False, *, keepdims: bool | bool[bool] | _NoValueType = ...) -> Any
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

Most cv2 image functions like `imread`, `cvtColor` return `cv2.mat_wrapper.Mat | NumPyArrayNumeric` , and `NumPyArrayNumeric` were defined as `ndarray[Any, dtype[integer[Any] | floating[Any]]]`  in cv2.tying.
It seems like this problem was occured because ty do not think `dtype[integer[Any] | floating[Any]]` equal to `dtype[integer[Any]] | dtype[floating[Any]]`

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Comment by @carljm on 2026-01-13 20:28_

I don't think that `dtype[integer[Any]] | dtype[floating[Any]]` should be assignable to `dtype[integer[Any] | floating[Any]]`.

My testing suggests this is an issue of real type incompatibility between cv2 stubs and numpy stubs, at certain versions. Given a particular version of opencv-python and numpy, I'm seeing the same behavior for ty and pyright. Given `opencv-python==4.11.0.86`, both ty and pyright are fine with your code on numpy 1.x, error on numpy >=2,<2.3, and are fine again on numpy 2.3+. It looks like the numpy type annotation was changed in 2.x a way that makes it incompatible with opencv, and then changed back again in 2.3.

Can you re-check  to ensure you are using the exact same numpy and opencv versions for testing pyright and testing ty? If you find a scenario where ty and pyright differ on this snippet, please provide the exact steps you are using to reproduce.

---

_Label `needs-info` added by @carljm on 2026-01-13 20:28_

---
