```yaml
number: 2537
title: Type context can be wrongly included in diagnostic type when there is no solution
type: issue
state: open
author: randolf-scholz
labels:
  - diagnostics
  - generics
  - bidirectional inference
assignees: []
created_at: 2026-01-16T20:33:40Z
updated_at: 2026-01-17T01:46:48Z
url: https://github.com/astral-sh/ty/issues/2537
synced_at: 2026-01-17T02:11:18Z
```

# Type context can be wrongly included in diagnostic type when there is no solution

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

_Comment by @carljm on 2026-01-17 01:45_

Just to be clear, this is purely an issue of the diagnostic / hover / revealed type -- there is no type checking correctness issue here?

I think this is a known issue with how we apply type context in generic cases. We would like to fix it, but we don't consider it super high priority since it just makes the revealed types sometimes wrongly include the context type, when there is no solution that satisfies the context type. It doesn't ever change the correctness of a program.

---

_Label `bidirectional inference` added by @carljm on 2026-01-17 01:45_

---

_Renamed from "Bad Type Variable resolution with outer context (`dict.get`)" to "Type context can be wrongly included in diagnostic type when there is no solution" by @carljm on 2026-01-17 01:45_

---

_Label `generics` added by @carljm on 2026-01-17 01:45_

---

_Comment by @carljm on 2026-01-17 01:46_

@ibraheemdev do we already have an issue open for this? If not, we can use this one.

---

_Label `diagnostics` added by @carljm on 2026-01-17 01:46_

---
