---
number: 2311
title: "invalid-method-override: silent on conflicting method signatures from multiple base classes"
type: issue
state: closed
author: eric-distyl-ai
labels: []
assignees: []
created_at: 2026-01-02T23:51:27Z
updated_at: 2026-01-02T23:58:08Z
url: https://github.com/astral-sh/ty/issues/2311
synced_at: 2026-01-10T01:51:14Z
---

# invalid-method-override: silent on conflicting method signatures from multiple base classes

---

_Issue opened by @eric-distyl-ai on 2026-01-02 23:51_

## Summary

ty's `invalid-method-override` rule does not detect conflicting method signatures when a class inherits from multiple base classes that define the same method with incompatible signatures. Pyright's `reportIncompatibleMethodOverride` catches this.

## Minimal Reproduction

```python
from typing import TypeVar, Generic

T = TypeVar("T")

class Mixin:
    def method(self, arg: object) -> None:
        pass

class Base(Generic[T]):
    def method(self, arg: T) -> None:
        pass

class Combined(Base[int], Mixin):
    pass
```

## Expected Behavior

Error: Base classes define method `method` in incompatible way.

- `Base[int].method` expects `arg: int`
- `Mixin.method` expects `arg: object`
- `object` is not assignable to `int`

This violates the Liskov Substitution Principle because the class cannot satisfy both method contracts.

## Actual Behavior

**ty 0.0.8**: "All checks passed!"

**pyright 1.1.390**:
```
error: Base classes for class "Combined" define method "method" in incompatible way
    Parameter 2 type mismatch: base parameter is type "object", override parameter is type "int"
      "object" is not assignable to "int" (reportIncompatibleMethodOverride)
```

## Why This Matters

This pattern is common in codebases using mixins with generic base classes. In our codebase, we have 91 instances of this error detected by pyright but missed by ty.

Example from real code:
```python
# ServerModule[TowerConfig].initialize expects config: TowerConfig
# DBMixin.initialize expects config: CONFIG_CLASS (unbound TypeVar → object)
class TowerModule(ServerModule[TowerConfig], DBMixin):  # Should error
    pass
```

## Environment

- ty version: 0.0.8
- Python: 3.11
- OS: macOS

---

_Comment by @eric-distyl-ai on 2026-01-02 23:58_

Closing - filed prematurely. Will re-evaluate after improving comparison methodology.

---

_Closed by @eric-distyl-ai on 2026-01-02 23:58_

---
