```yaml
number: 1541
title: "Subclass definitions should be validated against superclass `__init_subclass__` definitions"
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
assignees: []
created_at: 2025-11-13T20:20:16Z
updated_at: 2025-11-13T20:20:16Z
url: https://github.com/astral-sh/ty/issues/1541
synced_at: 2026-01-10T02:06:25Z
```

# Subclass definitions should be validated against superclass `__init_subclass__` definitions

---

_Issue opened by @AlexWaygood on 2025-11-13 20:20_

### Summary

The `Bar` definition here fails at runtime, but we currently don't catch the error:

```py
class Foo:
    def __init_subclass__(cls, arg: int): ...
    
class Bar(Foo): ...
```

```pytb
>>> class Bar(Foo): ...
... 
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    class Bar(Foo): ...
TypeError: Foo.__init_subclass__() missing 1 required positional argument: 'arg'
```

In order for it to succeed, it would need to be something like

```py
class Foo:
    def __init_subclass__(cls, arg: int): ...
    
class Bar(Foo, arg=42): ...
```

### Version

_No response_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-13 20:20_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-11-13 20:20_

---
