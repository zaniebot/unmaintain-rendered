```yaml
number: 1628
title: "`Literal` `str`s do not work with `Sequence[T]` generic function argument narrowing"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - generics
assignees: []
created_at: 2025-11-25T01:23:58Z
updated_at: 2025-12-02T02:06:37Z
url: https://github.com/astral-sh/ty/issues/1628
synced_at: 2026-01-12T15:54:25Z
```

# `Literal` `str`s do not work with `Sequence[T]` generic function argument narrowing

---

_@MeGaGiGaGon_

### Summary

Minimization of when I ran into this in my code:

https://play.ty.dev/21a7ef1b-756b-4981-af2d-878f5fe72ee4
```py
from typing import Sequence, assert_type

def first[T](x: Sequence[T]) -> T:
    return x[0]

reveal_type("1")  # Revealed type: `Literal["1"]`
assert_type(first("1"), str)  # Type `str` does not match asserted type `Unknown`
```
Since the type of `"1"` is defaulted to `Literal["1"]`, code that tries to rely on literal strings with this `Sequence` generic narrowing will not work.

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Renamed from "`str` does not work with `Sequence[str]` generic function argument narrowing" to "`Literal` `str`s do not work with `Sequence[T]` generic function argument narrowing" by @MeGaGiGaGon on 2025-11-25 01:27_

---

_Label `bug` added by @carljm on 2025-11-25 01:52_

---

_Added to milestone `Stable` by @carljm on 2025-11-25 01:52_

---

_Comment by @carljm on 2025-11-25 01:53_

Yes, this is odd, since `str` directly inherits `Sequence[str]`. It looks like we aren't falling back from `Literal["1"]` to `str` correctly?

---

_Comment by @MeGaGiGaGon on 2025-11-25 01:55_

I think it has something to do with the generic, since `static_assert(is_assignable_to(Literal["1"], Sequence[str]))` works as expected, so the generic is somehow causing the fallback to not apply/be tried/fail.

---

_Comment by @carljm on 2025-11-25 02:01_

Yes, we definitely understand the relationship between `Literal["1"]` and `str` in general, I mean that in the typevar solver we aren't falling back where we need to.

---

_Comment by @nklsla on 2025-11-29 11:12_

I think this one might be related to my issue.
I get an error, `ty[invalid-argument-type]`, from my function signature when using typehint. Removing the typehint resolves the error. Also defining the variable inside the function with or without the typehint results in no error either.
Im using `0.0.1a29`

```py
import polars as pl

def winsorize(
    col: pl.Expr | str,
    lower_quantile: float = 0.05,
    upper_quantile: float = 0.95,
    method: str = "linear", # Using typehint results in this error
    # method = "linear",  # No error, however not using typehint
) -> pl.Expr:
    """Winsorize a column by clipping its values to the specified quantiles."""

    if isinstance(col, str):
        _col = pl.col(col)
    elif isinstance(col, pl.Expr):
        _col = col
    else:
        raise TypeError("col must be a str or pl.Expr")

#  Re-/defining the value in function gives not error
#  method = "linear"  # no error
#  method: str = "linear" # no error
    return _col.clip(
        lower_bound=_col.quantile(lower_quantile, interpolation=method), # Argument to bound method `quantile` is incorrect: Expected `Literal["nearest", "higher", "lower", "midpoint", "linear", "equiprobable"]`, found `str`ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
        upper_bound=_col.quantile(upper_quantile, interpolation=method), # Argument to bound method `quantile` is incorrect: Expected `Literal["nearest", "higher", "lower", "midpoint", "linear", "equiprobable"]`, found `str`ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
    )
```

This is the signature for the `polars column` method `quantile`
```py
bound method Expr.quantile(
    quantile: int | float | Expr,
    interpolation: Literal["nearest", "higher", "lower", "midpoint", "linear", "equiprobable"] = Literal["nearest"]
) -> Expr
```




---

_Comment by @sharkdp on 2025-12-01 11:31_

> I get an error, `ty[invalid-argument-type]`, from my function signature when using typehint.

I don't think this is related to the issue here. If you annotate `method` as `str`, then ty *should* error on your code, because `Expr.quantile` expects `Literal["nearest", "higher", "lower", "midpoint", "linear", "equiprobable"]`. `str` is a supertype of this union of literals, and so you might accidentally pass invalid `str` values to `Expr.quantile`. You probably want to annotate your `method` parameter in the same way that `Expr.quantile`'s `interpolation` parameter is annotated.

---

_Label `generics` added by @carljm on 2025-12-02 02:06_

---
