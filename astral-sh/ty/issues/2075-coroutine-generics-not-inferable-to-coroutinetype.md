```yaml
number: 2075
title: Coroutine generics not inferable to CoroutineType
type: issue
state: open
author: LeoHxQin
labels: []
assignees: []
created_at: 2025-12-18T15:09:32Z
updated_at: 2026-01-08T17:58:27Z
url: https://github.com/astral-sh/ty/issues/2075
synced_at: 2026-01-12T15:54:26Z
```

# Coroutine generics not inferable to CoroutineType

---

_@LeoHxQin_

### Summary

```python
from typing import Callable, Coroutine, Any

def defer_execution[**P, T](func: Callable[P, T], *args: P.args, **kwargs: P.kwargs) -> Callable[P, T]:
    return func

def run_coroutine[T](co: Coroutine[Any, Any, T]) -> T:
    raise Exception()

async def do_nothing() -> None:
    return

# Argument to function `defer_execution` is incorrect: Expected `Coroutine[Any, Any, T@run_coroutine]`, found `CoroutineType[Any, Any, None]`
defer_execution(run_coroutine, do_nothing())
```

[ty playground](https://play.ty.dev/126bfe1c-c3f8-411b-93cf-844b61df2a35)

[pyright playground](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDSG5gCuMqdUAginAFB8Am1YFEHBqIAPrUAHtQDGLJGBQBtAFRqACvQAqAXQAUwJijkAuQqXJVqK7VH301REGgDOFzQDoX7p2oBrAHdfDyhvYNCASigAWgA%2BSzJKGjtdPTMeKGyoEGoYJhAUKGNTPh5RXJMJOUZFFFt9A1qLAjrWBpUuOHpu9JiEh0yc3KIkN2ooAFFpOWoEVmUDKPKiNzhTESERMAkUMBgAC1Q0ZbjEgDllamGcvIKigSFxKVkFRZQDEGra8Hr2fi7fZHE7LFZAA)

### Version

ty 0.0.2

---

_Added to milestone `M1` by @carljm on 2025-12-23 23:46_

---

_Comment by @carljm on 2025-12-23 23:47_

Thanks!

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-08 17:58_

---

_Added to milestone `Stable` by @carljm on 2026-01-08 17:58_

---
