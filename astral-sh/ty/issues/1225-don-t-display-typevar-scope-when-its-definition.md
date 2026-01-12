```yaml
number: 1225
title: "don't display TypeVar scope when its definition is also being displayed"
type: issue
state: open
author: KotlinIsland
labels:
  - diagnostics
  - generics
assignees: []
created_at: 2025-09-21T04:04:43Z
updated_at: 2025-09-22T12:31:53Z
url: https://github.com/astral-sh/ty/issues/1225
synced_at: 2026-01-12T15:54:24Z
```

# don't display TypeVar scope when its definition is also being displayed

---

_@KotlinIsland_

### Summary

```py
class A[C]:
    def f[F](self, x: C | F): ...

x: int = A.f  # Object of type `def f[F](self, x: Unknown | F@f) -> Unknown` is not assignable to `int`
```

excusing the `Unknown` here, but i would expect:

`def f[F](self, x: C@A | F) -> Unknown`

the definition of `F` is included in the display, so also showing the scope is redundant

### Version

_No response_

---

_Label `diagnostics` added by @AlexWaygood on 2025-09-22 07:51_

---

_Label `generics` added by @AlexWaygood on 2025-09-22 07:51_

---

_Renamed from "don't display type var scope when it's definition is also being displayed" to "don't display TypeVar scope when its definition is also being displayed" by @AlexWaygood on 2025-09-22 12:31_

---
