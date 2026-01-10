```yaml
number: 1809
title: "Prefer generic overload with matching positional-only parameters over `*args: Any` overload"
type: issue
state: closed
author: sharkdp
labels:
  - generics
  - overloads
assignees: []
created_at: 2025-12-08T15:30:50Z
updated_at: 2025-12-09T14:59:36Z
url: https://github.com/astral-sh/ty/issues/1809
synced_at: 2026-01-10T01:56:41Z
```

# Prefer generic overload with matching positional-only parameters over `*args: Any` overload

---

_Issue opened by @sharkdp on 2025-12-08 15:30_

Consider the following example, and notice how we currently infer `Unknown` for the call to `f(1, 2)`. This only happens if the `(*args: Any) -> tuple[Any, ...]` overload is also present, so I assume that the `Unknown` is caused by the fact that we can't eliminate all but one overload in the first stage of overload matching here, and then need to consider two overloads during typevar solving:

```py
from typing import overload, Any, reveal_type

@overload
def f[T](x: T, /) -> tuple[T]: ...
@overload
def f[T1, T2](x1: T1, x2: T2, /) -> tuple[T1, T2]: ...
@overload
def f(*args: Any) -> tuple[Any, ...]: ...
def f(*args: Any) -> tuple[Any, ...]:
    raise NotImplementedError

reveal_type(f(1))  # tuple[Literal[1]]
reveal_type(f(1, 2))  # Unknown
reveal_type(f(1, 2, 3))  # tuple[Any, ...]
```

What's also interesting is that we correctly infer `tuple[Literal[1], Literal[1]]` for a call to `f(1, 1)`, i.e. if `T1` and `T2` solve to the same type.

---

_Added to milestone `Beta` by @sharkdp on 2025-12-08 15:30_

---

_Label `generics` added by @sharkdp on 2025-12-08 15:30_

---

_Label `overloads` added by @sharkdp on 2025-12-08 15:30_

---

_Assigned to @dhruvmanila by @MichaReiser on 2025-12-08 16:55_

---

_Closed by @dhruvmanila on 2025-12-09 14:59_

---
