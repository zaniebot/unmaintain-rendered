```yaml
number: 1851
title: Cannot solve generics involving PEP 695 type aliases
type: issue
state: closed
author: sharkdp
labels:
  - generics
  - type aliases
assignees: []
created_at: 2025-12-11T09:16:04Z
updated_at: 2026-01-21T22:34:42Z
url: https://github.com/astral-sh/ty/issues/1851
synced_at: 2026-01-21T23:08:07Z
```

# Cannot solve generics involving PEP 695 type aliases

---

_@sharkdp_

We currently do not support generic PEP 695 aliases in the solver:

```py
type MyList[T] = list[T]

def head[T](my_list: MyList[T]) -> T:
    return my_list[0]

reveal_type(head([1, 2]))  # Unknown!
```
https://play.ty.dev/b453601a-9d2d-4a51-bde8-38f9c8de7d62


(it works fine with implicit and PEP 613 type aliases)

---

_Added to milestone `Stable` by @sharkdp on 2025-12-11 09:16_

---

_Label `generics` added by @sharkdp on 2025-12-11 09:16_

---

_Label `type aliases` added by @carljm on 2025-12-11 17:46_

---

_Renamed from "Can not solve generics involving PEP 695 type aliases" to "Cannot solve generics involving PEP 695 type aliases" by @AlexWaygood on 2026-01-08 18:55_

---

_Closed by @ibraheemdev on 2026-01-21 22:34_

---
