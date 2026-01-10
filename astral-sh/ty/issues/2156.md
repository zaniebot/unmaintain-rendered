---
number: 2156
title: "Extend `invalid-method-override` diagnostics to cover methods being overridden by non-methods"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-22T13:00:12Z
updated_at: 2025-12-22T17:26:11Z
url: https://github.com/astral-sh/ty/issues/2156
synced_at: 2026-01-10T01:52:52Z
---

# Extend `invalid-method-override` diagnostics to cover methods being overridden by non-methods

---

_Issue opened by @AlexWaygood on 2025-12-22 13:00_

We have a basic implementation of the Liskov Substitution Principle in place for method overrides. But we do not currently emit diagnostics for methods invalidly overridden by non-methods. We need to implement this.

This should include methods being overridden by definitions in the class body:

```py
class A:
	def f(self) -> None: ...

class B(A):
	f = None
```

It should include methods overridden by declarations in the class body:

```py
class A:
	def f(self) -> None: ...

class B(A):
	f: int
```

and it should include methods overridden by implicit instance attributes:

```py
class A:
	def f(self): ...

class B(A):
	def __init__(self) -> None:
        self.f = 42
```

We may want to fix https://github.com/astral-sh/ty/issues/1345 before fixing this issue.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-22 13:00_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-22 13:00_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-22 17:26_

---
