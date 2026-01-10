```yaml
number: 1157
title: "Fallback of recursive types to `Any` in return types of generic methods"
type: issue
state: open
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-09T14:38:24Z
updated_at: 2025-11-13T07:40:30Z
url: https://github.com/astral-sh/ty/issues/1157
synced_at: 2026-01-10T02:06:25Z
```

# Fallback of recursive types to `Any` in return types of generic methods

---

_Issue opened by @sharkdp on 2025-09-09 14:38_

### Summary

In the following snippet, the return type of `wrap` falls back to `Wrapper[Any | None]`. Note that this only happens if `wrap` itself is generic. When the (dummy) generic context `[S]` is removed, the return type is `Wrapper[Recursive]`, as expected:

```py
type Recursive = Recursive | None

class Wrapper[T]: ...

class C[T]:
    @staticmethod
    def wrap[S]() -> Wrapper[T]:
        return Wrapper()

reveal_type(C[Recursive].wrap())  # Wrapper[Any | None]
```
https://play.ty.dev/14d6e89f-8f51-4322-a862-c14799c67e9a

### Version

current main (25853e237)

---

_Label `bug` added by @sharkdp on 2025-09-09 14:38_

---

_Label `generics` added by @sharkdp on 2025-09-09 14:38_

---

_Comment by @carljm on 2025-09-09 16:42_

This is because currently `apply_type_mapping` falls back to `Any` when it encounters a recursive type.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 07:40_

---
