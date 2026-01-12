```yaml
number: 1357
title: Literal promotion is not performed for generic calls
type: issue
state: closed
author: ibraheemdev
labels:
  - bidirectional inference
assignees: []
created_at: 2025-10-14T20:44:56Z
updated_at: 2025-11-14T21:13:58Z
url: https://github.com/astral-sh/ty/issues/1357
synced_at: 2026-01-12T15:54:25Z
```

# Literal promotion is not performed for generic calls

---

_@ibraheemdev_

For example, on main:
```py
def lst[T](x: T) -> list[T]:
    return [x]

a = lst(1)
reveal_type(a)  # revealed: list[Literal[1]]
```

We currently perform literal promotion when inferring the list expression, i.e. before specialization. We may need a second literal promotion step during specialization for this to work, similar to what we do for generic constructors.

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-10-14 20:44_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:45_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-10-17 14:47_

---

_Closed by @ibraheemdev on 2025-11-14 21:13_

---
