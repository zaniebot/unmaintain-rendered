```yaml
number: 107
title: type parameters scope of a generic class is an eager scope (in a non-stub file)
type: issue
state: closed
author: carljm
labels:
  - control flow
  - runtime semantics
assignees: []
created_at: 2025-05-01T00:02:30Z
updated_at: 2025-08-15T15:00:56Z
url: https://github.com/astral-sh/ty/issues/107
synced_at: 2026-01-12T15:54:22Z
```

# type parameters scope of a generic class is an eager scope (in a non-stub file)

---

_@carljm_

Currently we treat the type parameters scope of a PEP 695 generic class as a lazy scope, even in a `.py` file:

```py
class C[T](C): ...
```

Which means the above example creates a cycle, when at runtime its simply a `NameError`.

We need to handle the cycle regardless, because this same code can exist in a stub file, where it really is cyclic. But in a non-stub file we should treat the type-parameter scope as eager, meaning the base-class reference `C` should just be an unresolved reference instead.

---

_Renamed from "[red-knot] type parameters scope of a generic class is an eager scope (in a non-stub file)" to "type parameters scope of a generic class is an eager scope (in a non-stub file)" by @MichaReiser on 2025-05-07 15:24_

---

_Label `control flow` added by @AlexWaygood on 2025-05-11 07:34_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:34_

---

_Closed by @carljm on 2025-08-15 15:00_

---
