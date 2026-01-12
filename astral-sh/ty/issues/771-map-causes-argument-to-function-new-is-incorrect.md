```yaml
number: 771
title: "map causes \"Argument to function `__new__` is incorrect\""
type: issue
state: closed
author: behrenhoff
labels:
  - generics
assignees: []
created_at: 2025-07-07T09:22:21Z
updated_at: 2025-10-06T18:50:10Z
url: https://github.com/astral-sh/ty/issues/771
synced_at: 2026-01-12T15:54:24Z
```

# map causes "Argument to function `__new__` is incorrect"

---

_@behrenhoff_

### Summary

Consider this code:
```py
import re

words = "hello;foo.bar".split(";")
# words = ["hello", "foo.bar"]  # no error

re_string = "|".join(map(re.escape, words))
# re_string = "|".join(re.escape(t) for t in words)  # no error

print(re_string)
```

In the `map` variant, this causes the error while the variant with the for-loop in join (commented out) passes the checks.

When you replace the `split` with `words = ["hello", "foo.bar"]`, the error is gone as well, so it also seems to be related to `str.split` in some way.

So my guess is something with LiteralString / AnyStr compatibility inside map is not working correctly.


The error message is:
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `__new__` is incorrect
 --> /tmp/ty.py:6:26
  |
4 | # words = ["hello", "foo.bar"]  # no error
5 |
6 | re_string = "|".join(map(re.escape, words))
  |                          ^^^^^^^^^ Expected `(LiteralString, /) -> Unknown`, found `def escape(pattern: AnyStr) -> AnyStr`
7 | # re_string = "|".join(re.escape(t) for t in words)  # no error
  |
info: Matching overload defined here
    --> stdlib/builtins.pyi:1602:13
     |
1600 |     else:
1601 |         @overload
1602 |         def __new__(cls, func: Callable[[_T1], _S], iterable: Iterable[_T1], /) -> Self: ...
     |             ^^^^^^^      ------------------------- Parameter declared here
1603 |         @overload
1604 |         def __new__(cls, func: Callable[[_T1, _T2], _S], iterable: Iterable[_T1], iter2: Iterable[_T2], /) -> Self: ...
     |
info: Non-matching overloads for function `__new__`:
info:   (cls, func: (_T1, _T2, /) -> _S, iterable: Iterable[_T1], iter2: Iterable[_T2], /) -> Self
info:   (cls, func: (_T1, _T2, _T3, /) -> _S, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], /) -> Self
info:   (cls, func: (_T1, _T2, _T3, _T4, /) -> _S, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], iter4: Iterable[_T4], /) -> Self
info:   (cls, func: (_T1, _T2, _T3, _T4, _T5, /) -> _S, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], iter4: Iterable[_T4], iter5: Iterable[_T5], /) -> Self
info:   (cls, func: (...) -> _S, iterable: Iterable[Any], iter2: Iterable[Any], iter3: Iterable[Any], iter4: Iterable[Any], iter5: Iterable[Any], iter6: Iterable[Any], /, *iterables: Iterable[Any]) -> Self
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

Note: when I tried to find similar bug reports I found https://github.com/astral-sh/ty/issues/358 - but considering that was closed in May, it might just be a related problem.

python --version
Python 3.12.3


### Version

ty 0.0.1-alpha.13

---

_Label `generics` added by @AlexWaygood on 2025-07-07 10:11_

---

_Comment by @sharkdp on 2025-07-14 13:49_

Thank you for reporting this.


> In the `map` variant, this causes the error while the variant with the for-loop in join (commented out) passes the checks.
> 
> When you replace the `split` with `words = ["hello", "foo.bar"]`, the error is gone as well, so it also seems to be related to `str.split` in some way.

No, I think this is related to the fact that we still infer `list[Unknown]` for list literals, whereas the type of `"hello;foo.bar".split(";")` is `list[LiteralString]`.


The core issue here seems to be that we do not support passing generic functions to another generic function? A smaller, self-contained example would be:

```py
from typing import Callable

def accepts_callable[T, S](func: Callable[[T], S], arg: T, /) -> S:
    return func(arg)

def int_identity(x: int) -> int:
    return x

accepts_callable(int_identity, 0)  # fine

def generic_identity[T](x: T) -> T:
    return x

accepts_callable(generic_identity, 0)  # Expected `(Literal[0], /) -> Unknown`, found `def generic_identity(x: T) -> T`
```
https://play.ty.dev/7b627915-028a-442b-a5e1-12b0bd7501de

---

_Comment by @carljm on 2025-07-18 23:13_

I suspect this is related to #95 

---

_Comment by @AlexWaygood on 2025-10-06 16:25_

Neither the original snippet nor @sharkdp's repro cause us to emit any diagnostics on the current `main` branch. Should we close this now, or is it worth adding one/both as a regression test?

---

_Comment by @carljm on 2025-10-06 18:49_

I don't think it's necessary to spend time adding regression tests specifically for this symptom. I suspect the ecosystem report on a breakage here would be quite significant, and that we already have regression tests for the underlying cause.

---

_Closed by @carljm on 2025-10-06 18:49_

---
