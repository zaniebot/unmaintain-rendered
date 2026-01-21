```yaml
number: 2546
title: "`invalid-argument-type` when passing overloaded function to `map`"
type: issue
state: open
author: auscompgeek
labels:
  - generics
  - overloads
assignees: []
created_at: 2026-01-17T12:39:07Z
updated_at: 2026-01-21T07:58:30Z
url: https://github.com/astral-sh/ty/issues/2546
synced_at: 2026-01-21T09:02:45Z
```

# `invalid-argument-type` when passing overloaded function to `map`

---

_@auscompgeek_

### Summary

On the following file foo.py:

```python
from typing import overload


@overload
def foo(x: int) -> str: ...

@overload
def foo(x: str) -> str: ...

def foo(x: int | str) -> str:
    return str(x)


def errors() -> None:
    for x in map(foo, range(1, 10)):
        print(x)
```

ty returns an error ([playground link](https://play.ty.dev/0d4b5ef1-33b2-455b-892d-17a6f09175e6)):

```console
> ty check foo.py
error[invalid-argument-type]: Argument to function `__new__` is incorrect
  --> foo.py:15:18
   |
14 | def errors() -> None:
15 |     for x in map(foo, range(1, 10)):
   |                  ^^^ Expected `(int | str, /) -> str | int`, found `Overload[(x: int) -> str, (x: str) -> str]`
16 |         print(x)
   |
info: Matching overload defined here
    --> stdlib/builtins.pyi:3866:13
     |
3864 |     else:
3865 |         @overload
3866 |         def __new__(cls, func: Callable[[_T1], _S], iterable: Iterable[_T1], /) -> Self: ...
     |             ^^^^^^^      ------------------------- Parameter declared here
3867 |         @overload
3868 |         def __new__(cls, func: Callable[[_T1, _T2], _S], iterable: Iterable[_T1], iter2: Iterable[_T2], /) -> Self: ...
     |
info: Non-matching overloads for function `__new__`:
info:   [_T1, _T2, Self](cls, func: (_T1, _T2, /) -> _S@map, iterable: Iterable[_T1], iter2: Iterable[_T2], /) -> Self
info:   [_T1, _T2, _T3, Self](cls, func: (_T1, _T2, _T3, /) -> _S@map, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], /) -> Self
info:   [_T1, _T2, _T3, _T4, Self](cls, func: (_T1, _T2, _T3, _T4, /) -> _S@map, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], iter4: Iterable[_T4], /) -> Self
info:   [_T1, _T2, _T3, _T4, _T5, Self](cls, func: (_T1, _T2, _T3, _T4, _T5, /) -> _S@map, iterable: Iterable[_T1], iter2: Iterable[_T2], iter3: Iterable[_T3], iter4: Iterable[_T4], iter5: Iterable[_T5], /) -> Self
info:   [Self](cls, func: (...) -> _S@map, iterable: Iterable[Any], iter2: Iterable[Any], iter3: Iterable[Any], iter4: Iterable[Any], iter5: Iterable[Any], iter6: Iterable[Any], /, *iterables: Iterable[Any]) -> Self
```

This results in an even more obscure error message when a class constructor is overloaded ([playground link](https://play.ty.dev/eebccddd-e66d-4f07-958a-cbc40b438037)), like:

```python
class Foo:
    @overload
    def __init__(self, x: int): ...
    @overload
    def __init__(self, x: str): ...
    def __init__(self, x) -> None:
        self.x = x
```

```console
> ty check foo.py
error[invalid-argument-type]: Argument to function `__new__` is incorrect
  --> foo.py:14:18
   |
13 | def errors() -> None:
14 |     for y in map(Foo, range(1, 10)):
   |                  ^^^ Expected `(int | str, /) -> Foo`, found `<class 'Foo'>`
15 |         print(y.x)
   |
info: Matching overload defined here
    --> stdlib/builtins.pyi:3866:13
[snip]
```

Possibly related issues, albeit closed:

- #822
- #771

### Version

ty 0.0.12

---

_Label `generics` added by @AlexWaygood on 2026-01-18 17:47_

---

_Label `overloads` added by @dhruvmanila on 2026-01-21 07:55_

---

_Comment by @dhruvmanila on 2026-01-21 07:58_

This seems specific to overloads because if I remove them then the error disappears.

Looking at the error message, I think it might be because the `func: Callable[[_T1], _S]` gets specialized into a single callable instead of capturing the overloads and so when the argument types are checked, there's a mismatch.

---
