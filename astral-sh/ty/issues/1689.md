```yaml
number: 1689
title: "Too narrow type for `pandas` `DataFrame(columns)`"
type: issue
state: closed
author: janosh
labels:
  - Protocols
assignees: []
created_at: 2025-11-30T20:51:27Z
updated_at: 2025-12-02T07:55:07Z
url: https://github.com/astral-sh/ty/issues/1689
synced_at: 2026-01-10T01:58:59Z
```

# Too narrow type for `pandas` `DataFrame(columns)`

---

_Issue opened by @janosh on 2025-11-30 20:51_

### Summary

passing `columns` as `list[str]` raises error in latest `ty`. started in 0.0.1-alpha.27 and affects 0.0.1-alpha.28 and 0.0.1-alpha.29 as well

```py
import pandas as pd

df = pd.DataFrame([[1, 2, 3], [4, 5, 6]], columns=["a", "b"])
```

> Argument to bound method `__init__` is incorrect: Expected `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 4 union elements`, found `list[Unknown | str]` `ty` [invalid-argument-type](https://ty.dev/rules#invalid-argument-type)

### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Label `Protocols` added by @sharkdp on 2025-12-01 09:10_

---

_Comment by @sharkdp on 2025-12-01 09:19_

Do you have `pandas-stubs` installed? Because it seems to work fine if that is available.

Without `pandas-stubs`, the type for `columns` is `Axes | None` where `Axes = ListLike` and
```
ListLike = Union[AnyArrayLike, SequenceNotStr, range]
```

`SequenceNotStr` is supposed to cover the `list` case. It is [a generic protocol](https://github.com/pandas-dev/pandas/blob/1895c382296bca720acc358b996f4800dbdf6c9c/pandas/_typing.py#L104-L128):
```py
# list-like

# from https://github.com/hauntsaninja/useful_types
# includes Sequence-like objects but excludes str and bytes
_T_co = TypeVar("_T_co", covariant=True)


class SequenceNotStr(Protocol[_T_co]):
    @overload
    def __getitem__(self, index: SupportsIndex, /) -> _T_co:
        ...

    @overload
    def __getitem__(self, index: slice, /) -> Sequence[_T_co]:
        ...

    def __contains__(self, value: object, /) -> bool:
        ...

    def __len__(self) -> int:
        ...

    def __iter__(self) -> Iterator[_T_co]:
        ...

    def index(self, value: Any, /, start: int = 0, stop: int = ...) -> int:
        ...

    def count(self, value: Any, /) -> int:
        ...

    def __reversed__(self) -> Iterator[_T_co]:
        ...
```


ty [won't let you assign `[[1, 2, 3], [4, 5, 6]]` to this protocol](https://play.ty.dev/8791e13a-fb76-41da-a47d-d6a718740a82), and ty is actually correct. The problem is the `index` method in that protocol:
```py
    def index(self, value: Any, /, start: int = 0, stop: int = ...) -> int:
        ...
```

If we compare that to `list.index` from typeshed, we see the problem:
```py
    def index(self, value: _T, start: SupportsIndex = 0, stop: SupportsIndex = sys.maxsize, /) -> int:
        """Return first index of value.

        Raises ValueError if the value is not present.
        """
```

The protocol only accepts an `index` method with keyword-or-positional `start` and `stop` parameters, but `list.index`'s `start` and `stop` parameters are positional-only. So I think this needs to be fixed in `pandas`.

In ty, however, we should definitely work on our `invalid-assignment` diagnostics! Ideally, the error message should have pointed this out.

---

_Comment by @sharkdp on 2025-12-01 09:20_

Oh, apparently this has been fixed in `pandas` main already: https://github.com/pandas-dev/pandas/issues/56995#issuecomment-1902728314

---

_Comment by @kalekseev on 2025-12-01 09:35_

@sharkdp I have a similar problem trying to use ty with DRF typings that started from 27 alpha, should I create a separate issue for that? 

```py
from __future__ import annotations

from typing import TYPE_CHECKING, Literal

if TYPE_CHECKING:
    from collections.abc import Sequence


type _Method = Literal["GET", "POST", "PUT", "DELETE"]


def test(method: Sequence[_Method]) -> None:
    return None


test(["POST"])
```


```bash
uvx ty check  a.py
error[invalid-argument-type]: Argument to function `test` is incorrect
  --> a.py:16:6
   |
16 | test(["POST"])
   |      ^^^^^^^^ Expected `Sequence[_Method]`, found `list[Unknown | str]`
   |
info: Function defined here
  --> a.py:12:5
   |
12 | def test(method: Sequence[_Method]) -> None:
   |     ^^^^ ------------------------- Parameter declared here
13 |     return None
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

---

_Comment by @sharkdp on 2025-12-01 09:37_

@kalekseev Yes, please open a new ticket. => see https://github.com/astral-sh/ty/issues/1699

---

_Comment by @carljm on 2025-12-02 02:54_

Seems like this is an issue in pandas (and/or with not using pandas-stubs), not an issue in ty.

---

_Closed by @carljm on 2025-12-02 02:54_

---

_Comment by @carljm on 2025-12-02 02:55_

@sharkdp This felt familiar 😆 https://github.com/astral-sh/ty/issues/1591#issuecomment-3554468014

---

_Comment by @sharkdp on 2025-12-02 07:55_

> [@sharkdp](https://github.com/sharkdp) This felt familiar 😆 [#1591 (comment)](https://github.com/astral-sh/ty/issues/1591#issuecomment-3554468014)

Maybe I'll remember it better next time, now that I've gone through the exercise myself 🙈

---
