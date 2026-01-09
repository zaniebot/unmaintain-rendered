---
number: 20353
title: RUF009 - Specific attribute value definition should be valid
type: issue
state: open
author: IDrokin117
labels:
  - type-inference
assignees: []
created_at: 2025-09-11T17:50:14Z
updated_at: 2025-09-11T19:06:10Z
url: https://github.com/astral-sh/ruff/issues/20353
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF009 - Specific attribute value definition should be valid

---

_Issue opened by @IDrokin117 on 2025-09-11 17:50_

### Summary

Consider the following attribute value definitions as valid for [function-call-in-dataclass-default-argument (RUF009)](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/#function-call-in-dataclass-default-argument-ruf009)

1.  Nested descriptor definition (related to https://github.com/astral-sh/ruff/issues/20266). [Playground](https://play.ruff.rs/ad2a8c0b-a3e7-4fcb-b86a-c1fc48f3af02)
```python
import dataclasses


class Descriptor:

    def __get__(self, instance, owner):
        pass

    def __set__(self, instance, value):
        pass


@dataclasses.dataclass(frozen=True)
class A:
    class NestedDescriptor:

        def __get__(self, instance, owner):
            pass

        def __set__(self, instance, value):
            pass

    descriptor: Descriptor = Descriptor()
    nested_descriptor: NestedDescriptor = NestedDescriptor() # RUF009 Do not perform function call `NestedDescriptor` in dataclass defaults
```

2. Non-direct dataclass instantiation. [Playground](https://play.ruff.rs/a5acb0de-57ea-44f3-8150-56609d0a1af5)
```python
import dataclasses


@dataclasses.dataclass(frozen=True)
class B:
    attr: int = 1

class Temp:
    cls = B

@dataclasses.dataclass(frozen=True)
class A:
    b: B = B()
    temp_b: B = Temp.cls()  # RUF009 Do not perform function call `Temp.cls` in dataclass defaults
```
3. Import from module
```python
# my_module.py
import dataclasses

@dataclasses.dataclass(frozen=True)
class C:
    attr: int = 1
```
```python
#main.py
import dataclasses
from my_module import C
import my_module


@dataclasses.dataclass(frozen=True)
class B:
    attr: int = 1


@dataclasses.dataclass(frozen=True)
class A:
    b: B = B()
    imported_c: C = C()  # RUF009 Do not perform function call `C` in dataclass defaults
    imported_module_c: C = my_module.C()  # RUF009 Do not perform function call `my_module.C` in dataclass defaults

```

### Version

ruff 0.12.12

---

_Label `type-inference` added by @ntBre on 2025-09-11 19:06_

---
