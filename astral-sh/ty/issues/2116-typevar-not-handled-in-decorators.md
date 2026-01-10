```yaml
number: 2116
title: TypeVar not handled in decorators
type: issue
state: closed
author: mxmpl
labels: []
assignees: []
created_at: 2025-12-19T16:30:55Z
updated_at: 2025-12-20T16:14:41Z
url: https://github.com/astral-sh/ty/issues/2116
synced_at: 2026-01-10T01:56:41Z
```

# TypeVar not handled in decorators

---

_Issue opened by @mxmpl on 2025-12-19 16:30_

### Summary

I found this because the return type of `pl.read_csv` in `polars` is inferred as `Unknown` instead of `pl.DataFrame`: https://github.com/pola-rs/polars/blob/2a151c10fa76790711c2f75e6d012dd69c627ddd/py-polars/src/polars/io/csv/functions.py#L46-L49.

I reduced it to this example:

```py
from collections.abc import Callable
from typing import TypeVar, reveal_type

T = TypeVar("T")


def decorator() -> Callable[[Callable[[], T]], Callable[[], T]]:
    def decorate(function: Callable[[], T]) -> Callable[[], T]:
        return function

    return decorate


def foo() -> None: ...


@decorator()
def bar() -> None: ...


reveal_type(foo) # Revealed type: `def foo() -> None` (revealed-type) [Ln 21, Col 13]
reveal_type(bar) # Revealed type: `() -> Unknown` (revealed-type) [Ln 22, Col 13]
```

Both mypy and pyright infer the type of the decorated `bar` as `() -> None`.

Playground link: https://play.ty.dev/f073ef61-ba7f-4af2-a171-5f0c77caf754

### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Comment by @carljm on 2025-12-20 16:14_

Thanks! This is already tracked at #1136 

---

_Closed by @carljm on 2025-12-20 16:14_

---
