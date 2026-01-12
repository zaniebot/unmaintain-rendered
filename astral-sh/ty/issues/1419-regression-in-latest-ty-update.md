```yaml
number: 1419
title: Regression in latest ty update
type: issue
state: closed
author: Matt-Ord
labels: []
assignees: []
created_at: 2025-10-23T13:49:36Z
updated_at: 2025-12-12T18:50:58Z
url: https://github.com/astral-sh/ty/issues/1419
synced_at: 2026-01-12T15:54:25Z
```

# Regression in latest ty update

---

_@Matt-Ord_

### Summary

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable
    from pathlib import Path


class CachedFunction[**P, R]:  # noqa: B903
    """A function wrapper which is used to cache the output."""

    def __init__(
        self,
        function: Callable[P, R],
        path: Path | Callable[P, Path | None] | None,
    ) -> None:
        self._inner = function
        self.Path = path

    def _get_cache_path(self, *args: P.args, **kw: P.kwargs) -> Path | None:
        cache_path = self.Path(*args, **kw) if callable(self.Path) else self.Path
        if cache_path is None:
            return None
        return cache_path
```

Todo & !None should be assignable to Path |None

```
error[invalid-return-type]: Return type does not match returned value
  --> thesis_calculations/temp.py:21:65
   |
19 |         self.Path = path
20 |
21 |     def _get_cache_path(self, *args: P.args, **kw: P.kwargs) -> Path | None:
   |                                                                 ----------- Expected `Path | None` because of return type
22 |         cache_path = self.Path(*args, **kw) if callable(self.Path) else self.Path
23 |         if cache_path is None:
24 |             return None
25 |         return cache_path
   |                ^^^^^^^^^^ expected `Path | None`, found `(@Todo & ~None) | (Path & ~(() -> object)) | (((...) -> Path | None) & ~(() -> object))`
   |
info: rule `invalid-return-type` is enabled by default

Found 2 diagnostics
```

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-10-23 19:33_

Thanks for the report. The issue here isn't the `Todo & ~None` element of the union -- we do indeed consider this to be assignable to `Path | None`. [Here's a demo in the playground](https://play.ty.dev/74c785c5-11ae-4ea4-abd4-425e8d9ac34c) that shows this (the demo uses `Any` rather than `Todo`, but `Any` and `Todo` are always treated the same by us internally).

It's the `(((...) -> Path | None) & ~(() -> object))` element of the union that causes us to complain that the returned object isn't assignable to the return type annotation. That _should_ have been narrowed away by this line in your code:

```py
cache_path = self.Path(*args, **kw) if callable(self.Path) else self.Path
```

but our narrowing for `if callable` isn't quite corect at the moment.

Apologies for the jargon, but: the ultimate cause of this is that `builtins.callable` in typeshed is annotated as returning `TypeIs[Callable[..., Any]]`, and our implementation of `TypeIs` implicitly converts `Callable[..., Any]` to the top-materialization of that type, and our implementation of the top materialization for `Callable` types is currently buggy due to [this TODO](https://github.com/astral-sh/ruff/blob/83a3bc4ee94de552d5cec9a3146aff00dade6903/crates/ty_python_semantic/src/types/signatures.rs#L1448-L1453)

---

_Comment by @Matt-Ord on 2025-10-23 19:48_

Ah no problem feel free to close if it's a known issue 

---

_Comment by @AlexWaygood on 2025-10-23 20:04_

It was a known issue, but we didn't have a GitHub issue tracking it. I've written up a more focussed one now (https://github.com/astral-sh/ty/issues/1426); I'll close this in favour of that one, since it'll be easier to monitor what needs to be done that way.

Thanks again for the report!

---

_Closed by @AlexWaygood on 2025-10-23 20:04_

---

_Reopened by @carljm on 2025-12-12 18:50_

---

_Closed by @carljm on 2025-12-12 18:50_

---
