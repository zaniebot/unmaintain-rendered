```yaml
number: 1982
title: typing.Never return function yields incorrect type inference
type: issue
state: closed
author: SadoP
labels: []
assignees: []
created_at: 2025-12-17T08:16:12Z
updated_at: 2025-12-17T08:28:46Z
url: https://github.com/astral-sh/ty/issues/1982
synced_at: 2026-01-12T15:54:26Z
```

# typing.Never return function yields incorrect type inference

---

_@SadoP_

### Summary

Hei

Suppose you have a variable with a union type, check for the type at runtime and raise a `TypeError` if it's the incorrect type. This is correctly recognized by ty. If you replace the raise statement with a function with return type `Never`, for example if you have some repetitive error handling outsourced there, ty does not recognize that the type of the variable has changed. Here's a code snippet, I've used mypy to verify that the typing is actually correct.

[https://play.ty.dev/46fdf394-2c5f-4f42-9388-e3d1b60b535f](https://play.ty.dev/46fdf394-2c5f-4f42-9388-e3d1b60b535f)
```python
from typing import Never, reveal_type


def raise_err() -> Never:
    raise TypeError


def foo(val: str | int) -> int:
    if isinstance(val, str):
        raise TypeError
    reveal_type(val)
    return val


def bar(val: str | int) -> int:
    if isinstance(val, str):
        raise_err()
    reveal_type(val)
    return val
```

Here's the output from ty check:

```
info[revealed-type]: Revealed type
  --> ty_test.py:11:17
   |
 9 |     if isinstance(val, str):
10 |         raise TypeError
11 |     reveal_type(val)
   |                 ^^^ `int`
12 |     return val
   |

info[revealed-type]: Revealed type
  --> ty_test.py:18:17
   |
16 |     if isinstance(val, str):
17 |         raise_err()
18 |     reveal_type(val)
   |                 ^^^ `str | int`
19 |     return val
   |

error[invalid-return-type]: Return type does not match returned value
  --> ty_test.py:15:28
   |
15 | def bar(val: str | int) -> int:
   |                            --- Expected `int` because of return type
16 |     if isinstance(val, str):
17 |         raise_err()
18 |     reveal_type(val)
19 |     return val
   |            ^^^ expected `int`, found `str | int`
   |
info: rule `invalid-return-type` is enabled by default

Found 3 diagnostics
```

And here the output from mypy on the same file:
```
ty_test.py:11: note: Revealed type is "builtins.int"
ty_test.py:18: note: Revealed type is "builtins.int"
Success: no issues found in 1 source file
```


### Version

ty 0.0.2

---

_Comment by @AlexWaygood on 2025-12-17 08:21_

Thanks! It's high-priority for us to fix this. Please see #690 

---

_Closed by @AlexWaygood on 2025-12-17 08:21_

---

_Comment by @SadoP on 2025-12-17 08:28_

Oh, sorry for the duplicate in that case. I've searched but didn't find that particular issue.

---
