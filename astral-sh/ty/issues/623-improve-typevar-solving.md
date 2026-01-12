```yaml
number: 623
title: improve typevar solving
type: issue
state: open
author: DetachHead
labels:
  - generics
assignees: []
created_at: 2025-06-10T11:56:31Z
updated_at: 2025-12-10T18:05:46Z
url: https://github.com/astral-sh/ty/issues/623
synced_at: 2026-01-12T15:54:23Z
```

# improve typevar solving

---

_@DetachHead_

### Summary

```py
def safe[T](value: T | None) -> T: ...


def _(value: int | None):
    reveal_type(safe(value)) # int | None
```
https://play.ty.dev/8370e070-c8b0-430a-8e42-b645793e5561

it looks like `T` is being inferred as `int | None`, but it would be more convenient if it was inferred as `int`, so that the `safe` function can behave sort of like a type guard that removes `None` from types.

both [pyright](https://basedpyright.com/?typeCheckingMode=all&code=CYUwZgBAzghmIG0AqBdAFANxgGwK4gC4IkIAfCAOQHsA7EASggFoA%2BYogOi4ChfRIA%2Bphz4iASxoAXMpVoMC3CEogAnEBhA4BkgJ4AHEGljxheBowDEECZKA) and [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=c87e6bb90e256b9e3dfd15db83195ff4) support inferring the generics this way. 

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `generics` added by @AlexWaygood on 2025-06-10 12:09_

---

_Comment by @dcreager on 2025-06-10 18:09_

Our specialization inference is very limited at the moment, [especially for union types](https://github.com/astral-sh/ruff/blob/6051a118d16355b0c5a5d4ff763b383b7e88c064/crates/ty_python_semantic/src/types/generics.rs#L674-L683). This is a good example of something that should be handled by the full constraint solver that we're planning on adding.

---

_Added to milestone `Beta` by @carljm on 2025-07-23 22:57_

---

_Comment by @carljm on 2025-07-23 22:58_

Doesn't seem like we have an open issue for better constraint solver, so repurposing this one for that.

---

_Renamed from "removing a type from a union using a generic doesn't work" to "improve typevar solving" by @carljm on 2025-07-23 22:59_

---

_Assigned to @dcreager by @dcreager on 2025-07-24 01:11_

---

_Comment by @AlexWaygood on 2025-11-05 17:44_

Here's a minimized version of https://github.com/astral-sh/ty/issues/1481, which is something we should aim to support with the new constraint solver:

```py
from typing import Protocol, reveal_type

class SupportsNext[T](Protocol):
    def __next__(self) -> T:
        raise NotImplementedError

class HasNext:
    def __next__(self) -> int:
        return 42

def solve_my_typevar_please[T](x: SupportsNext[T]) -> T:
    raise NotImplementedError

def _(x: HasNext):
    reveal_type(solve_my_typevar_please(x))  # Currently `Unknown`, should be `int`
```

Here `HasNext` only implicitly implements `SupportsNext` (it does not explicitly subclass `SupportsNext`), so the missing support for this with the current solver relates to [this TODO here](https://github.com/astral-sh/ruff/blob/eda85f3c646ddb9f3dddf13315d653f51d187f64/crates/ty_python_semantic/src/types/generics.rs#L1551-L1559)

---

_Removed from milestone `Beta` by @MichaReiser on 2025-12-05 15:50_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-05 15:50_

---

_Comment by @andythomas on 2025-12-10 09:03_

I do not know, if this adds something new, but even to judge that gets too difficult for me... 

I have two issues: The incorrect error message from ty that `a.error` is "Not an instance or subclass of `BaseException`", which might or might not be caused by the narrowing of `a` "(Success[str] & Top[Error[Unknown]]) | Error[FileNotFoundError]"

https://play.ty.dev/ae688c4a-c03d-4b8d-9b08-f0494ec8f7d4



```python
from dataclasses import dataclass
from typing import Generic, TypeAlias, TypeVar

T = TypeVar("T")
E = TypeVar("E")


@dataclass(frozen=True)
class Success(Generic[T]):
    value: T


@dataclass(frozen=True)
class Error(Generic[E]):
    error: E


Result: TypeAlias = Success[T] | Error[E]


def find_binary(binary: str) -> Result[str, FileNotFoundError]:
    if binary == "b":
        return Success("yes.")
    else:
        return Error(FileNotFoundError("File not found."))


a = find_binary("a")
if isinstance(a, Error):
    raise a.error
```





---

_Comment by @sharkdp on 2025-12-10 09:25_

@andythomas Your problem is not really related to generics. If you look at the narrowed type of `a` inside the `isinstance` branch, you'll notice that it is `(Success[str] & Top[Error[Unknown]]) | Error[FileNotFoundError]`, which shows that it's not as narrow as you might think. The first part of the union (`Success[str] & Top[Error[Unknown]]`) accounts for the possibility of common subclasses of `Success` and `Error`. To avoid this, you could make one or both of `Success` and `Error` `@final`, in which case the "Cannot raise object of type" error disappears.

---

_Comment by @andythomas on 2025-12-10 09:37_

Excellent, that works as intended. Thank you for the very quick help!

---
