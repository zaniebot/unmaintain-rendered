```yaml
number: 2473
title: Missing inference for overloaded decorator function with optional params
type: issue
state: closed
author: keatingw
labels: []
assignees: []
created_at: 2026-01-13T01:22:29Z
updated_at: 2026-01-13T01:28:57Z
url: https://github.com/astral-sh/ty/issues/2473
synced_at: 2026-01-13T02:20:57Z
```

# Missing inference for overloaded decorator function with optional params

---

_@keatingw_

### Summary

When using an overloaded decorator that can be used both bare (`@deco`) and with arguments (`@deco()`), `ty` correctly infers the type for the bare usage but fails for the parenthesized call.

I've seen this with libraries like `tenacity` making use of heavily-parameterized decorators but can replicate with the below example. This cuts across a few pieces with `ParamSpec`, `overload` and the decoration/nesting of `Callable` so I'm not 100% sure on the cause.

```python
from typing import Callable, TypeVar, ParamSpec, overload, reveal_type

P = ParamSpec("P")
R = TypeVar("R")

@overload
def deco(fn: Callable[P, R]) -> Callable[P, R]: ...

@overload
def deco(*, arg: int = 0) -> Callable[[Callable[P, R]], Callable[P, R]]: ...

def deco(
    fn: Callable[P, R] | None = None,
    *,
    arg: int = 0
) -> Callable[P, R] | Callable[[Callable[P, R]], Callable[P, R]]:
    def decorator(fn: Callable[P, R]) -> Callable[P, R]:
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            print("decorated with args")
            return fn(*args, **kwargs)
        return wrapper

    if fn is not None:
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            print("decorated")
            return fn(*args, **kwargs)
        return wrapper
    return decorator

@deco
def func1(x: int)->int:
    return x

@deco()
def func2(x: int)->int:
    return x

reveal_type(func1)  # Revealed type: `(x: int) -> int`
reveal_type(func2)  # Revealed type: `(...) -> Unknown`
```

[Playground link](https://play.ty.dev/9cf502eb-ae92-45f9-b271-6792cae4876d)

mypy and [Pyright](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQAqi1AakSPQArtEQDKCagGN6YAG7UQJMEQAm9ENQmkA%2BvEEAoDZygBeKNxC8BwgBQAiTuYCUGgEp7GzNiAt2bWgALjJ0uRtlqYChAoTBTYBQALkJScipqAG1OejsAXWsoAFoAPliyShpk1LSYgDoKrx8pGVkAoJDhcIAqenY0GNR8fQAGTNz8%2BKLE4gKE4qh0tPpRoaSUybTSqAqyrUDg0PCNKF2oSJjZwvmSqAAfKAA5MBRqR2vb2h291ufd9s6UbqgejX68o7jBbpc6DY6JEZxcHApYzKFAkqlN6NTZNIwwXARaJghGLf44oowqLIvYoqAAdyMCEErma7QAzjFOGUGfRms0ANbkhlMspchn4uzE0kivYIEBdCxbdHUWQUpAwAAWUAZHlFosUMAAriAUPsUKY6SA0PS2Zzucb6bZ1VBNTq9ZSiNTJFpSUhgpFMPSoCgwPgHtRheqNhSqTTDTyDCzLWb%2BZbeXGTYKgzbduLJeZpUQYLK1andnbdfqIzGoOzE1aSXtCw6wy7STXGmF0bgvFt6h6tSghABGUwAD0%2BMGsuS6Kdt1G1Rf7baaplsIeAXaEACYB0ORzkx8jGzONIplCQ1MwIsue7YD9RVOpqKfuyvrEA) both correctly infer the same `(x: int) -> int` for both cases

Thanks so much for your great work on `ty`!

### Version

Playground (3ae4db3cc)

---

_Renamed from "Inference issue for overloaded decorator function with optional params" to "Missing inference for overloaded decorator function with optional params" by @keatingw on 2026-01-13 01:24_

---

_Comment by @carljm on 2026-01-13 01:28_

Thanks for the report! This is https://github.com/astral-sh/ty/issues/1136

---

_Closed by @carljm on 2026-01-13 01:28_

---
