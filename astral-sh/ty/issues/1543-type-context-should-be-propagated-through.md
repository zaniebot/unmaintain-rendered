```yaml
number: 1543
title: Type context should be propagated through conditional expressions
type: issue
state: closed
author: ibraheemdev
labels:
  - bidirectional inference
assignees: []
created_at: 2025-11-14T02:04:01Z
updated_at: 2025-11-14T20:19:09Z
url: https://github.com/astral-sh/ty/issues/1543
synced_at: 2026-01-12T15:54:25Z
```

# Type context should be propagated through conditional expressions

---

_@ibraheemdev_

For example, this should not error:
```py
def f[T](x: T) -> list[T]:
    raise NotImplementedError

# error: Object of type `list[Literal[1]]` is not assignable to `list[int | None]`
x: list[int | None] = f(1) if True else f(2)
```

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-11-14 02:04_

---

_Closed by @ibraheemdev on 2025-11-14 20:19_

---
