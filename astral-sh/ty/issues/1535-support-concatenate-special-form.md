```yaml
number: 1535
title: Support Concatenate special form
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-11-12T23:51:53Z
updated_at: 2025-12-15T19:35:14Z
url: https://github.com/astral-sh/ty/issues/1535
synced_at: 2026-01-12T15:54:25Z
```

# Support Concatenate special form

---

_@carljm_

https://typing.python.org/en/latest/spec/generics.html#paramspec

---

_Added to milestone `Stable` by @carljm on 2025-11-12 23:51_

---

_Assigned to @dhruvmanila by @carljm on 2025-11-12 23:51_

---

_Label `typing semantics` added by @dhruvmanila on 2025-11-13 10:41_

---

_Comment by @vlashada on 2025-12-14 08:54_

`ty` fails to infer the return type (`T`) for a generic method whose first parameter is a callable typed with `Callable[Concatenate[SelfType, P], T]`. In practice this shows up in `polars.DataFrame.pipe`, where `reveal_type(df.pipe(f))` becomes `Unknown` even when `f` is fully annotated to return `DataFrame`.

Polars documents `DataFrame.pipe` as:

```py
DataFrame.pipe(
    function: Callable[Concatenate[DataFrame, P], T],
    *args: P.args,
    **kwargs: P.kwargs,
) -> T
```

## Reproduction (no third-party deps)

https://play.ty.dev/60c9407f-0310-4b4e-b078-fba3d381f132

```py
from typing import Callable, Concatenate, ParamSpec, TypeVar, reveal_type

P = ParamSpec("P")
T = TypeVar("T")

class DF:
    def pipe(self, function: Callable[Concatenate["DF", P], T], *args: P.args, **kwargs: P.kwargs) -> T:
        return function(self, *args, **kwargs)

def f(df: DF) -> DF:
    return df

reveal_type(DF())          # expected: DF
reveal_type(DF().pipe(f))  # expected: DF; actual (ty): Unknown
````

## Reproduction (real-world: Polars)

```py
import polars as pl
from typing import reveal_type

def f(df: pl.DataFrame) -> pl.DataFrame:
    return df

reveal_type(pl.DataFrame())          # expected: DataFrame
reveal_type(pl.DataFrame().pipe(f))  # expected: DataFrame; actual (ty): Unknown
```

---

_Comment by @dhruvmanila on 2025-12-15 05:37_

@vlashada Thanks for the report. The issue that you're highlighting is actually https://github.com/astral-sh/ruff/pull/21551. I tried out your example (the "no third-party deps" one) and it works:

```
info[revealed-type]: Revealed type
  --> /Users/dhruv/playground/ty/bug-report/main.py:21:13
   |
21 | reveal_type(DF())  # expected: DF
   |             ^^^^ `DF`
22 | reveal_type(DF().pipe(f))  # expected: DF; actual (ty): Unknown
   |

info[revealed-type]: Revealed type
  --> /Users/dhruv/playground/ty/bug-report/main.py:22:13
   |
21 | reveal_type(DF())  # expected: DF
22 | reveal_type(DF().pipe(f))  # expected: DF; actual (ty): Unknown
   |             ^^^^^^^^^^^^ `DF`
   |

Found 2 diagnostics
```

---

_Comment by @dhruvmanila on 2025-12-15 09:05_

Related to https://github.com/astral-sh/ruff/pull/21958, it might be useful to try modeling `classmethod` using typeshed's definition once `Concatenate` support has landed because it's used to define the callable type as `Callable[Concatenate[type[_T], _P], _R_co]`.

---

_Comment by @carljm on 2025-12-15 19:35_

After looking more at classmethod recently for https://github.com/astral-sh/ruff/pull/21958, I am skeptical that we will be able to implement classmethod support solely using the typeshed definition -- but I suspect we could use it more than we do today.

---
