```yaml
number: 2537
title: "Bad Type Variable resolution with outer context (`dict.get`)"
type: issue
state: open
author: randolf-scholz
labels: []
assignees: []
created_at: 2026-01-16T20:33:40Z
updated_at: 2026-01-16T20:33:40Z
url: https://github.com/astral-sh/ty/issues/2537
synced_at: 2026-01-16T21:03:54Z
```

# Bad Type Variable resolution with outer context (`dict.get`)

---

_@randolf-scholz_

### Summary

Consider a `dict[K, V]` like class with a get method:

```python
from typing import Any, reveal_type

class Map[K, V]:
    def set(self, key: K, value: V) -> None: ...
    def get[T=None](self, key: Any, default: T = None, /) -> V | T: ...

d: Map[str, int] = Map[str, int]()

# as expected, these are both `Any | None`
reveal_type(d.get("key"))  # int | None  ✅️
reveal_type(d.get("key", None))  # int | None  ✅️
# outer context should not overrule the argument type!
result: str = reveal_type(d.get("key", None))  # int | None | str ❌️
```

`d.get("key", None)` alone produces `int | None` as expected, but in the last case we have `str` as outer context. This gives an unsolvable situation for the type variable `T` as we would need both `T <: str` and `T :> None`. It seems `ty` gives priority to the outer constraint `T <: str` here, which seems misguided, as it produces an illogical `reveal_type` result: `int | None | str`.

But `d.get("key", None)` cannot produce a `str`!

Instead, the inner constraint `T :> None` should probably be prioritized in this situation.

See also mirror issues in `mypy` and `pyrefly` tracker: https://github.com/python/mypy/issues/20576, https://github.com/facebook/pyrefly/issues/2136






### Version

_No response_

---
