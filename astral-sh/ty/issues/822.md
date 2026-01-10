```yaml
number: 822
title: "Expression involving `map` and `os.path.realpath`"
type: issue
state: closed
author: dalcinl
labels: []
assignees: []
created_at: 2025-07-14T13:02:37Z
updated_at: 2025-07-14T13:29:58Z
url: https://github.com/astral-sh/ty/issues/822
synced_at: 2026-01-10T02:07:36Z
```

# Expression involving `map` and `os.path.realpath`

---

_Issue opened by @dalcinl on 2025-07-14 13:02_

### Summary

I'm not really sure how to classify this issue.

Minimal reproducer:

``` python
# file: tmp.py
import os
import typing
path = os.environ["PATH"].split(os.path.pathsep)
for p in map(os.path.realpath, path):
    print(p)
```

```console
$ ty check tmp.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `__new__` is incorrect
 --> tmp.py:4:14
  |
2 | import typing
3 | path = os.environ["PATH"].split(os.path.pathsep)
4 | for p in map(os.path.realpath, path):
  |              ^^^^^^^^^^^^^^^^ Expected `(str, /) -> Unknown`, found `Overload[(filename: PathLike[AnyStr], *, strict: bool | @Todo = Literal[False]) -> AnyStr, (filename: AnyStr, *, strict: bool | @Todo = Literal[False]) -> AnyStr]`
5 |     print(p)
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

### Version

0.0.1-alpha.14


---

_Comment by @sharkdp on 2025-07-14 13:20_

Thank you for reporting this.

The issue seems to be that we don't recognize `realpath` as being assignable to `Callable[[str], Unknown]`. This can also be demonstrated [like this](https://play.ty.dev/bb091958-4f78-44a5-8e6c-66bf269b3439).



---

_Comment by @sharkdp on 2025-07-14 13:27_

I think this may actually be the same issue as #771. Will close this as a duplicate and update the other ticket.

---

_Closed by @sharkdp on 2025-07-14 13:27_

---
