```yaml
number: 1356
title: Call expression type context should affect function argument inference
type: issue
state: closed
author: ibraheemdev
labels:
  - bidirectional inference
assignees: []
created_at: 2025-10-14T20:36:46Z
updated_at: 2025-11-12T19:20:00Z
url: https://github.com/astral-sh/ty/issues/1356
synced_at: 2026-01-12T15:54:25Z
```

# Call expression type context should affect function argument inference

---

_@ibraheemdev_

Currently, the only type context we have access to when inferring function arguments is the annotated parameter type. However, the call expression annotation is also relevant, e.g.,
```py
def id[T](x: T) -> T:
    return x

def _(i: int):
    x: list[int | None] = id([i])
    reveal_type(x)   # revealed (main): list[int | None] | list[Unknown | int] 
```

The problem is that we currently infer function arguments before the specializing the function call, and so the type context when inferring the list expression is still a typevar. On the other hand, we use the inferred argument types to infer the specialization, so there's a bit of a circular dependency here.

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-10-14 20:36_

---

_Comment by @ibraheemdev on 2025-10-17 03:34_

Interestingly, this particular example would be resolved by https://github.com/astral-sh/ty/issues/136, but something like a `TypedDict` annotation would not.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:45_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-10-17 14:47_

---

_Closed by @ibraheemdev on 2025-11-12 19:20_

---
