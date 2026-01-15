```yaml
number: 2479
title: "`no-matching-overload` when using `cv2` and `numpy`"
type: issue
state: closed
author: Carrot-shreds
labels:
  - needs-info
assignees: []
created_at: 2026-01-13T10:56:34Z
updated_at: 2026-01-14T23:54:44Z
url: https://github.com/astral-sh/ty/issues/2479
synced_at: 2026-01-15T00:41:56Z
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

_Comment by @carljm on 2026-01-13 22:19_

Going to go ahead and close to keep the tracker actionable, but will happily reopen if you do have an example showing ty behavior differing on the same numpy/cv versions!

---

_Closed by @carljm on 2026-01-13 22:19_

---

_Comment by @Carrot-shreds on 2026-01-14 09:00_

Thank you for your testing! It's indeed a problem related to these dependency version, instead of the type checker.

I'm using python3.13 with the latest `opencv-python>=4.12.0.88`, which lock the maximum verion of numpy to `numpy<2.3.0`, so my numpy was degrade to `numpy>=2.2.6` automatically. And it should be fixed in the next cv2 release.
After i change my dependencies to `opencv-python>=4.11.0.86`, `numpy>=2.4.1`, the error was disappeared.

About the difference of ty and pyright. Sorry I made a mistake that i didn't actually test the sample code with pyright, I just write another similar case as a sample and ty check it. I retest the sample with `numpy==2.2.6` and `opencv-python==4.12.0.88` using both pyright and ty. They all give the same "no-matching-overload" error as your result.

But i also retest my actual project code, they do act differently.
(In a new uv 3.13.9 env with numpy==2.2.6 and opencv-python==4.12.0.88)
```
import cv2
import numpy as np
...
def func(img: np.ndarray):
    if len(img.shape) != 2:
        img = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
    if np.average(img) < 128:
        img = 255 - img
    ...
```
```
PS G:\code\Python\tytest> uvx pyright --pythonpath .venv/Scripts/python main.py
0 errors, 0 warnings, 0 informations
```
And `uvx ty check main.py` will got the same error as the sample code.

I also found if I change the code like this, pyright will also give the error like ty.
```
import cv2
import numpy as np
...
def func(image_in: np.ndarray):
    if len(image_in.shape) != 2:
        img = cv2.cvtColor(image_in, cv2.COLOR_RGB2GRAY)
    else:
        img = image_in
    if np.average(img) < 128:
        img = 255 - img
    ...
```
```
PS G:\code\Python\tytest> uvx pyright --pythonpath .venv/Scripts/python main.py
g:\code\Python\tytest\main.py
  g:\code\Python\tytest\main.py:12:8 - error: No overloads for "average" match the provided arguments (reportCallIssue)
  g:\code\Python\tytest\main.py:12:19 - error: Argument of type "MatLike | ndarray[Unknown, Unknown]" cannot be assigned to parameter "a" of type "_ArrayLikeComplex_co | _ArrayLikeObject_co" in function "average"
    Type "MatLike | ndarray[Unknown, Unknown]" is not assignable to type "_ArrayLikeComplex_co | _ArrayLikeObject_co"
      Type "NumPyArrayNumeric" is not assignable to type "_ArrayLikeComplex_co | _ArrayLikeObject_co"
        "ndarray[Any, dtype[integer[Any] | floating[Any]]]" is incompatible with protocol "_SupportsArray[dtype[numpy.bool[builtins.bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]"
          "__array__" is an incompatible type
            No overloaded function matches type "() -> ndarray[Any, _DType_co@_SupportsArray]"
        "ndarray[Any, dtype[integer[Any] | floating[Any]]]" is incompatible with protocol "_NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool]] | dtype[integer[Any]] | dtype[floating[Any]] | dtype[complexfloating[Any, Any]]]]"
          "__reversed__" is not present
          "count" is not present
    ... (reportArgumentType)
2 errors, 0 warnings, 0 informations
```

I think the behaviour difference perhaps because pyright's default `typeCheckingMode="standard"` is less strict than ty's strategy.
Anyway, change the numpy version do solve the problem. Thanks again for teams wonderful working!

---

_Comment by @carljm on 2026-01-14 23:54_

Thanks for the additional analysis! Yes, it looks like the difference in behavior between pyright and mypy on your new example is because pyright loses some more specific type information from the call to `cv2.cvtColor`, because of the original `img: np.ndarray` annotation. When you switch to using two separate variables, pyright no longer loses that information and emits the same error as ty.

---
