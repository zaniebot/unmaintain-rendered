```yaml
number: 290
title: "Too many positional arguments to bound method: expected 0, got 0"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
  - diagnostics
  - calls
assignees: []
created_at: 2025-05-09T11:20:30Z
updated_at: 2025-05-15T02:51:24Z
url: https://github.com/astral-sh/ty/issues/290
synced_at: 2026-01-12T15:54:22Z
```

# Too many positional arguments to bound method: expected 0, got 0

---

_@InSyncWithFoo_

Minimal reproducible example ([playground](https://play.ty.dev/afd4f49f-a6a8-4b31-86e2-de0072c1f88e)):

```python
class A:
    def foo(): ...  # No `self`

A().foo()  # Too many positional arguments to bound method `foo`: expected 0, got 0
```

Arguably, the "got 0" part should be "got 1 (including `self`)".

---

_Label `bug` added by @AlexWaygood on 2025-05-09 11:22_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-09 11:22_

---

_Label `calls` added by @AlexWaygood on 2025-05-10 18:03_

---

_Closed by @carljm on 2025-05-15 02:51_

---
