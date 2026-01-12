```yaml
number: 1977
title: "Generic type parameter incorrectly inferred from `Callable[[S], T | None]` as `T | None` instead of `T`"
type: issue
state: open
author: fadedDexofan
labels:
  - generics
assignees: []
created_at: 2025-12-17T05:47:08Z
updated_at: 2025-12-17T07:45:03Z
url: https://github.com/astral-sh/ty/issues/1977
synced_at: 2026-01-12T15:54:26Z
```

# Generic type parameter incorrectly inferred from `Callable[[S], T | None]` as `T | None` instead of `T`

---

_@fadedDexofan_

## Summary

When a generic class has two parameters that share a type variable `T`:
- `extractor: Callable[[S], T | None]`
- `inner: Specification[T]`

ty incorrectly infers `T` from the Callable's return type as `T | None`, ignoring the concrete `T` from the `Specification[T]` argument.

**Expected:** `T = int` (from `GreaterThanSpec` which is `Specification[int]`)
**Actual:** `T = int | None` (from `Callable[[Container], int | None]`)

mypy correctly infers `T = int`.

## Minimal Reproduction
[Ty playground link](https://play.ty.dev/dc5ccb2a-a953-4fa2-bef6-96cdd466a1bb)
[MyPy playground link](https://mypy-play.net/?mypy=latest&python=3.14&flags=strict&gist=a1a4f9b57096163feedf6302549b4e69)

```python
from abc import ABC, abstractmethod
from collections.abc import Callable


class Specification[T](ABC):
    @abstractmethod
    def is_satisfied_by(self, candidate: T, /) -> bool:
        pass

    def via[S](self, extractor: Callable[[S], T | None]) -> Specification[S]:
        return LiftSpec(extractor, self)


class LiftSpec[S, T](Specification[S]):
    def __init__(
        self,
        extractor: Callable[[S], T | None],
        inner: Specification[T],
    ) -> None:
        self._extractor = extractor
        self._inner = inner

    def is_satisfied_by(self, candidate: S, /) -> bool:
        extracted = self._extractor(candidate)
        return extracted is not None and self._inner.is_satisfied_by(extracted)


class GreaterThanSpec(Specification[int]):
    def __init__(self, threshold: int) -> None:
        self._threshold = threshold

    def is_satisfied_by(self, candidate: int, /) -> bool:
        return candidate > self._threshold


class Container:
    def __init__(self, value: int | None) -> None:
        self.value = value


def extract_value(c: Container) -> int | None:
    return c.value


# All three should work - T should be inferred as `int` from GreaterThanSpec
spec1 = LiftSpec(extract_value, GreaterThanSpec(5))  # ty error
spec2 = GreaterThanSpec(10).via(extract_value)       # ty error  
spec3 = LiftSpec[Container, int](extract_value, GreaterThanSpec(5))  # works with explicit params

reveal_type(spec1)
reveal_type(spec2)
reveal_type(spec3)
```

## ty output

```
error[invalid-argument-type]: Argument to bound method `__init__` is incorrect
  --> repro.py:26:16
   |
25 |     def via[S](self, extractor: Callable[[S], T | None]) -> Specification[S]:
26 |         return LiftSpec(extractor, self)
   |                         ^^^^^^^^^ Expected `(S@via, /) -> None`, found `(S@via, /) -> T@Specification | None`

error[invalid-argument-type]: Argument to bound method `__init__` is incorrect
  --> repro.py:26:27
   |
26 |         return LiftSpec(extractor, self)
   |                                    ^^^^ Expected `Specification[None]`, found `Self@via`

error[invalid-argument-type]: Argument to bound method `__init__` is incorrect
  --> repro.py:51:32
   |
51 | spec1 = LiftSpec(extract_value, GreaterThanSpec(5))
   |                                 ^^^^^^^^^^^^^^^^^^ Expected `Specification[int | None]`, found `GreaterThanSpec`
```

## mypy output

Passes with no errors. `reveal_type` shows correct inference:
```
Revealed type is "LiftSpec[Container, int]"
```

## Version

```
Python 3.14.2
ty 0.0.2 (42835578d 2025-12-16)
mypy 1.19.1
```

---

_Renamed from "Generic type parameter incorrectly inferred from Callable[[S], T | None] as T | None instead of T" to "Generic type parameter incorrectly inferred from `Callable[[S], T | None]` as `T | None` instead of `T`" by @fadedDexofan on 2025-12-17 05:47_

---

_Label `generics` added by @AlexWaygood on 2025-12-17 07:44_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 07:45_

---
