```yaml
number: 2554
title: "Class type variables solved to `Unknown` if a generic class has a constructor method that is generic over different type variables"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - generics
assignees: []
created_at: 2026-01-18T12:36:16Z
updated_at: 2026-01-18T12:36:16Z
url: https://github.com/astral-sh/ty/issues/2554
synced_at: 2026-01-18T13:20:36Z
```

# Class type variables solved to `Unknown` if a generic class has a constructor method that is generic over different type variables

---

_@AlexWaygood_

We support this pattern:

```py
class Foo[T]:
    def __init__(self: Foo[T], x: T) -> None: ...

reveal_type(Foo(42))  # revealed: Foo[Literal[42]]
```

but we do not support this pattern:

```py
class Foo[T]:
    def __init__[S](self: Foo[S], x: S) -> None: ...

reveal_type(Foo(42))  # revealed: Foo[Unknown]
```

The typing spec conformance suite has several tests that will not pass unless this is fixed.

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-18 12:36_

---

_Label `bug` added by @AlexWaygood on 2026-01-18 12:36_

---

_Label `generics` added by @AlexWaygood on 2026-01-18 12:36_

---
