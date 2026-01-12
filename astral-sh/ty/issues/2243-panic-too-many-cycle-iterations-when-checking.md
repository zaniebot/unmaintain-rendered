```yaml
number: 2243
title: "[panic] too many cycle iterations when checking Callable type alias with narwhals.LazyFrame"
type: issue
state: open
author: matthiaskaeding
labels:
  - fatal
assignees: []
created_at: 2025-12-27T22:54:47Z
updated_at: 2026-01-09T13:44:36Z
url: https://github.com/astral-sh/ty/issues/2243
synced_at: 2026-01-12T15:54:26Z
```

# [panic] too many cycle iterations when checking Callable type alias with narwhals.LazyFrame

---

_@matthiaskaeding_

## Description

`ty check` panics with "too many cycle iterations" when checking code that uses a `Callable` type alias with `narwhals.LazyFrame` as parameter and return types.

## Minimal Reproduction

```python
"""Minimal reproduction for ty panic."""
from typing import Callable

import narwhals as nw

Builder = Callable[[nw.LazyFrame], nw.LazyFrame]


def get_builder() -> Builder:
    return _builder


def _builder(df: nw.LazyFrame) -> nw.LazyFrame:
    native = df._compliant_frame.native
    return nw.from_native(native).lazy()
```

Save as `repro.py` and run:
```bash
ty check repro.py
```

## Error Output

```
error[panic]: Panicked at .../execute.rs:321:21 when checking `repro.py`: `is_redundant_with_impl(Id(...)): execute: too many cycle iterations`
info: This indicates a bug in ty.
```

<details>
<summary>Full stack trace</summary>

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:321:21 when checking `/tmp/ty_repro10.py`: `is_redundant_with_impl(Id(a058)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: Platform: macos aarch64
info: Version: 0.0.4 (c1e6188b1 2025-12-18)
info: Args: ["ty", "check", "/tmp/ty_repro10.py"]
info: query stacktrace:
   0: infer_definition_types(Id(cdf0))
             at crates/ty_python_semantic/src/types/infer.rs:103
   1: infer_deferred_types(Id(ce09))
             at crates/ty_python_semantic/src/types/infer.rs:145
   2: TypeVarInstance < 'db >::lazy_bound_(Id(9409))
             at crates/ty_python_semantic/src/types.rs:9708
   3: inferable_typevars_inner(Id(4d09))
             at crates/ty_python_semantic/src/types/generics.rs:336
             cycle heads: TypeVarInstance < 'db >::lazy_bound_(Id(9408)) -> iteration = 200, TypeVarInstance < 'db >::lazy_bound_(Id(9407)) -> iteration = 200
   4: infer_definition_types(Id(cf3a))
             at crates/ty_python_semantic/src/types/infer.rs:103
   5: cached_protocol_interface(Id(b8cb))
             at crates/ty_python_semantic/src/types/protocol_class.rs:870
   6: is_equivalent_to_object_inner(Id(a409))
             at crates/ty_python_semantic/src/types/instance.rs:699
   7: infer_deferred_types(Id(f5c9))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: is_equivalent_to_object_inner(Id(a404)) -> iteration = 200, cached_protocol_interface(Id(b84b)) -> iteration = 200, cached_protocol_interface(Id(b832)) -> iteration = 200, TypeVarInstance < 'db >::lazy_bound_(Id(9407)) -> iteration = 200
   8: FunctionType < 'db >::signature_(Id(4451))
             at crates/ty_python_semantic/src/types/function.rs:839
   9: infer_definition_types(Id(f5c9))
             at crates/ty_python_semantic/src/types/infer.rs:103
  10: cached_protocol_interface(Id(b84b))
             at crates/ty_python_semantic/src/types/protocol_class.rs:870
             cycle heads: infer_definition_types(Id(f5c5)) -> iteration = 200
  11: infer_deferred_types(Id(f5c5))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: is_equivalent_to_object_inner(Id(a404)) -> iteration = 200
  12: FunctionType < 'db >::signature_(Id(444f))
             at crates/ty_python_semantic/src/types/function.rs:839
  13: infer_definition_types(Id(f5c5))
             at crates/ty_python_semantic/src/types/infer.rs:103
  14: cached_protocol_interface(Id(b832))
             at crates/ty_python_semantic/src/types/protocol_class.rs:870
  15: is_equivalent_to_object_inner(Id(a404))
             at crates/ty_python_semantic/src/types/instance.rs:699
  16: infer_definition_types(Id(cdf1))
             at crates/ty_python_semantic/src/types/infer.rs:103
  17: infer_definition_types(Id(cdf3))
             at crates/ty_python_semantic/src/types/infer.rs:103
  18: infer_deferred_types(Id(ce0a))
             at crates/ty_python_semantic/src/types/infer.rs:145
  19: TypeVarInstance < 'db >::lazy_bound_(Id(9408))
             at crates/ty_python_semantic/src/types.rs:9708
  20: infer_definition_types(Id(cdee))
             at crates/ty_python_semantic/src/types/infer.rs:103
  21: infer_deferred_types(Id(ce05))
             at crates/ty_python_semantic/src/types/infer.rs:145
  22: TypeVarInstance < 'db >::lazy_bound_(Id(9407))
             at crates/ty_python_semantic/src/types.rs:9708
  23: infer_definition_types(Id(680b))
             at crates/ty_python_semantic/src/types/infer.rs:103
  24: ClassLiteral < 'db >::implicit_attribute_inner_(Id(b406))
             at crates/ty_python_semantic/src/types/class.rs:1524
  25: Type < 'db >::member_lookup_with_policy_(Id(2c19))
             at crates/ty_python_semantic/src/types.rs:881
  26: infer_definition_types(Id(1405))
             at crates/ty_python_semantic/src/types/infer.rs:103
  27: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:69
  28: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:534
```

</details>

## Environment

- ty version: 0.0.4 (c1e6188b1 2025-12-18)
- Platform: macOS aarch64 (Darwin 24.6.0)
- Python: 3.11.12
- narwhals: 2.1.2

## Notes

The panic appears to be triggered by the combination of:
1. A `Callable` type alias using `nw.LazyFrame` as both parameter and return type
2. A function returning an instance of that Callable type
3. The function body accessing `df._compliant_frame.native` and calling `nw.from_native(...).lazy()`

Removing any of these elements causes the panic to disappear.

---

_Renamed from "Panic: too many cycle iterations when checking Callable type alias with narwhals.LazyFrame" to "[panic] too many cycle iterations when checking Callable type alias with narwhals.LazyFrame" by @matthiaskaeding on 2025-12-27 22:56_

---

_Label `fatal` added by @mtshiba on 2025-12-28 05:51_

---

_Comment by @MeGaGiGaGon on 2025-12-28 17:59_

This minimization was very painful, but I think I pulled everything out into one file.

One of the annoying things was this was fixed on later versions of narwhals, so I initially couldn't reproduce.

The crash only happens on 3.14 instead of 3.11 because I could not get the statement orders to work without lazy names, but it should still be the same crash at it's core.

This code:

<details>
<summary>Collapsed because long</summary>

```py
from enum import auto, Enum
from collections.abc import Sequence, Callable
from typing import Protocol, Self, Any, TypeVar, TypeAlias

CompliantDataFrameAny: TypeAlias = "CompliantDataFrame[Any]"
CompliantLazyFrameAny: TypeAlias = "CompliantLazyFrame[Any, Any, Any]"
CompliantFrameAny: TypeAlias = "CompliantDataFrameAny | CompliantLazyFrameAny"
CompliantFrameT_co = TypeVar(
    "CompliantFrameT_co", bound=CompliantFrameAny, covariant=True
)
CompliantFrameT = TypeVar("CompliantFrameT", bound=CompliantFrameAny)
EvalNames: TypeAlias = Callable[[CompliantFrameT], Sequence[str]]
CompliantExprAny: TypeAlias = "CompliantExpr[Any]"
CompliantExprT = TypeVar("CompliantExprT", bound=CompliantExprAny)
ExprT = TypeVar("ExprT", bound=CompliantExprAny)
CompliantSeriesAny: TypeAlias = "CompliantSeries[Any]"
CompliantSeriesOrNativeExprAny: TypeAlias = "CompliantSeriesAny | NativeExpr"
SeriesT = TypeVar("SeriesT", bound=CompliantSeriesOrNativeExprAny)
FrameT = TypeVar("FrameT", bound=CompliantFrameAny)
CompliantSeriesOrNativeExprT = TypeVar(
    "CompliantSeriesOrNativeExprT", bound=CompliantSeriesOrNativeExprAny
)
EvalSeries: TypeAlias = Callable[
    [CompliantFrameT], Sequence[CompliantSeriesOrNativeExprT]
]

class CompliantNamespace(Protocol[CompliantFrameT, CompliantExprT]):
    _implementation: Implementation
    _version: Version

    def when(
        self, predicate: CompliantExprT
    ) -> CompliantWhen[CompliantFrameT, Any, CompliantExprT]: ...

IntoExpr: TypeAlias = "SeriesT"
class CompliantWhen(Protocol[FrameT, SeriesT, ExprT]):

    def then(
        self, value: IntoExpr[SeriesT, ExprT], /
    ) -> CompliantThen[FrameT, SeriesT, ExprT, Self]:
        return self._then.from_when(self, value)

WhenT_contra = TypeVar(
    "WhenT_contra", bound=CompliantWhen[Any, Any, Any], contravariant=True
)
class NativeExpr(Protocol): ...
class Version(Enum):
    V1 = auto()
    V2 = auto()
    MAIN = auto()

class CompliantColumn(Protocol):
    """Common parts of `Expr`, `Series`."""

    _version: Version

    def __add__(self, other: Any) -> Self: ...

    def __narwhals_namespace__(self) -> CompliantNamespace[Any, Any]: ...

class CompliantExpr(
    CompliantColumn, Protocol[CompliantFrameT]
):
    def __narwhals_namespace__(self) -> CompliantNamespace[CompliantFrameT, Self]: ...
    @classmethod
    def from_column_names(
        cls,
        evaluate_column_names: EvalNames[CompliantFrameT],
    ) -> Self: ...
    @classmethod
    def from_column_indices(
        cls, *column_indices: int
    ) -> Self: ...
    @staticmethod
    def _eval_names_indices(indices: Sequence[int], /) -> EvalNames[CompliantFrameT]:
        def fn(df: CompliantFrameT) -> Sequence[str]:
            column_names = df.columns
            return [column_names[i] for i in indices]

        return fn

    def all(self) -> Self: ...
    def any(self) -> Self: ...
    def count(self) -> Self: ...
    def min(self) -> Self: ...

class CompliantThen(
    CompliantExpr[FrameT], Protocol[FrameT, SeriesT, ExprT, WhenT_contra]
):
    _call: EvalSeries[FrameT, SeriesT]

class CompliantGroupBy(Protocol):

    @property
    def compliant(self) -> CompliantFrameT_co: ...

CompliantExprT_contra = TypeVar(
    "CompliantExprT_contra", bound=CompliantExprAny, contravariant=True
)

class CompliantDataFrame(
    Protocol[CompliantExprT_contra],
):
    def with_columns(self, *exprs: CompliantExprT_contra) -> Self: ...


class CompliantLazyFrame(
    Protocol,
):
    def group_by(
        self,
    ) -> CompliantGroupBy: ...
```

</details>

Crashes with the stack trace:

<details>
<summary>Collapsed because long</summary>

```
error[panic]: Panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\ce80691\src\function\execute.rs:321:21 when checking `D:\python_projects\repro-narwhals\issue.py`: `infer_deferred_types(Id(772d)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Version: 0.0.7 (cf82a04b5 2025-12-24)
info: Args: ["C:\\Users\\GiGaGon\\AppData\\Local\\uv\\cache\\archive-v0\\x1LGjCZA-slfQngMhuLzR\\Scripts\\ty.exe", "check", "--python-version", "3.14", "issue.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: TypeVarInstance < 'db >::lazy_bound_(Id(8403))
             at crates\ty_python_semantic\src\types.rs:9753
   1: infer_definition_types(Id(7711))
             at crates\ty_python_semantic\src\types\infer.rs:103
   2: infer_deferred_types(Id(7728))
             at crates\ty_python_semantic\src\types\infer.rs:145
   3: TypeVarInstance < 'db >::lazy_bound_(Id(8402))
             at crates\ty_python_semantic\src\types.rs:9753
   4: infer_definition_types(Id(7714))
             at crates\ty_python_semantic\src\types\infer.rs:103
   5: infer_definition_types(Id(7716))
             at crates\ty_python_semantic\src\types\infer.rs:103
   6: infer_deferred_types(Id(772e))
             at crates\ty_python_semantic\src\types\infer.rs:145
   7: TypeVarInstance < 'db >::lazy_bound_(Id(8401))
             at crates\ty_python_semantic\src\types.rs:9753
   8: is_redundant_with_impl(Id(9002))
             at crates\ty_python_semantic\src\types.rs:2037
   9: infer_deferred_types(Id(140c))
             at crates\ty_python_semantic\src\types\infer.rs:145
  10: infer_scope_types(Id(1000))
             at crates\ty_python_semantic\src\types\infer.rs:69
  11: check_file_impl(Id(c00))
             at crates\ty_project\src\lib.rs:548
```

</details>

Good luck to anyone else that wants to look into this/minimize it further, I'm out of energy myself. I didn't look into it closely but it's probably something related to recursive protocols.

---

_Comment by @dangotbanned on 2025-12-28 22:47_

> Good luck to anyone else that wants to look into this/minimize it further, I'm out of energy myself. I didn't look into it closely but it's probably something related to recursive protocols.

I'm [likely to blame](https://github.com/narwhals-dev/narwhals/pulls?q=is%3Apr+is%3Aclosed+label%3Atyping) for the typing in `narwhals` 😅

Indeed, I mentioned on our discord recently that I didn't expect `ty` to work for us yet due to the fiddly protocols.

But for this example, there are a couple of odd things as well:

1. `nw.LazyFrame` is generic, but every usage here would be `nw.LazyFrame[Unknown]`
2. `nw.LazyFrame._compliant_frame.native` is not public API - it should be using `nw.LazyFrame.to_native`
3. `nw.from_native` is heavily overloaded, and IIRC `ty` doesn't support all the features I used in the most recent update to them



---

_Comment by @MeGaGiGaGon on 2025-12-28 23:25_

I don't think the original example was the cause of the crash, at least I hope. The `narwhals` library itself does have at least one crash when you run ty on it, so it would be very difficult if not impossible to figure out if this example was just exposing ty to that original crash, or if it's actually it's own unique thing.

Also just for some more context on the minimized version, how I do them is I get a copy of the source code, run the example, and delete code until the crash stops happening, which is why some of the classes are missing methods/bases/generic arguments. That's also why I'm not 100% certain that it's the exact same issue, just that it is an issue. Since it's also a cycle based crash any change to the source code can change stack/message, so there's that too. The minimized version doesn't include the original example since when I was copying all the code from different files into one file I noticed that eventually ty crashed without having run through the example, so I stopped there.

---

_Comment by @mtshiba on 2025-12-29 07:19_

~~With lazy evaluation of implicit union aliases (astral-sh/ruff#22238 addresses this), these examples no longer appear to panic.~~ No, I tried again and the panic persisted.

---

_Comment by @MichaReiser on 2025-12-29 08:23_

@MeGaGiGaGon thank you this is very useful. I pointed Claude at it and it was able to minimize the problem further:

```py
"""Minimal reproduction for ty cycle crash (20 lines).
 
Regression test for https://github.com/astral-sh/ty/issues/2243
 
The crash requires this specific combination:
1. A TypeVar with a forward-reference union bound: `TypeVar("T", bound="X[Any] | int")`
2. A Callable type alias using that TypeVar: `TypeAlias = Callable[[T], None]`
3. A Protocol using Self in a method returning another Protocol with that TypeVar
4. A subclass Protocol inheriting from the parent with the TypeVar
5. Another Protocol using ExprT (bounded by forward ref) in a method
 
Run with: ty check --python-version 3.14
Expected: Should not crash with "too many cycle iterations"
"""
from collections.abc import Callable
from typing import Protocol, Self, Any, TypeVar, TypeAlias
 
FrameT = TypeVar("FrameT", bound="Frame[Any] | int")
Fn: TypeAlias = Callable[[FrameT], None]
ExprT = TypeVar("ExprT", bound="Expr[Any]")
 
class NS(Protocol[FrameT, ExprT]):
    def when(self) -> Then[FrameT]: ...
 
class Expr(Protocol[FrameT]):
    def ns(self) -> NS[FrameT, Self]: ...
    @classmethod
    def col(cls, f: Fn[FrameT]) -> Self: ...
 
class Then(Expr[FrameT], Protocol[FrameT]):
    pass
 
class Frame(Protocol[ExprT]):
    def with_columns(self, e: ExprT) -> None: ...
```

What's worth noting is that both MREs are slightly different from the original report as they panic in `infer_deferred_types` and not `is_redundant_with_impl`

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-29 19:54_

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:44_

---
