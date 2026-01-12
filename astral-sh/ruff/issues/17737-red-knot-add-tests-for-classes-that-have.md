```yaml
number: 17737
title: "[red-knot] Add tests for classes that have incompatible `__new__` and `__init__` methods"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2025-04-30T14:24:53Z
updated_at: 2025-04-30T20:40:17Z
url: https://github.com/astral-sh/ruff/issues/17737
synced_at: 2026-01-12T15:54:56Z
```

# [red-knot] Add tests for classes that have incompatible `__new__` and `__init__` methods

---

_@AlexWaygood_

It doesn't look like we yet have any test cases for classes where `__new__` and `__init__` are incompatible with each other. Construction of these classes always fails at runtime, but we should make sure we handle them correctly anyway. (It looks like we _do_ handle them correctly, but that doesn't mean we shouldn't add tests for it.)

```py
import abc

class Foo:
    def __new__(cls) -> "Foo":
        return object.__new__(cls)
    def __init__(self, x):
        self.x = 42

Foo()  # fails due to incorrect arguments to `__init__`
Foo(42)  # fails due to incorrect arguments to `__new__`

class Foo2:
    def __new__(cls, x) -> "Foo2":
        return object.__new__(cls)
    def __init__(self):
        pass

Foo2()  # fails
Foo2(42)  # also fails

class Foo3(metaclass=abc.ABCMeta):
    def __new__(cls) -> "Foo3":
        return object.__new__(cls)
    def __init__(self, x):
        self.x = 42

Foo3()  # fails
Foo3(42)  # also fails

class Foo4(metaclass=abc.ABCMeta):
    def __new__(cls, x) -> "Foo4":
        return object.__new__(cls)
    def __init__(self):
        pass

Foo4()  # fails
Foo4(42)  # also fails
```

These tests should be added to `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`.

---

_Label `help wanted` added by @AlexWaygood on 2025-04-30 14:24_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-30 14:24_

---

_Label `testing` added by @AlexWaygood on 2025-04-30 14:24_

---

_Comment by @LaBatata101 on 2025-04-30 14:35_

Can you assign me to this one?

---

_Assigned to @LaBatata101 by @AlexWaygood on 2025-04-30 14:37_

---

_Closed by @carljm on 2025-04-30 20:40_

---
