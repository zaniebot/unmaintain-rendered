```yaml
number: 1897
title: "`typing.Self` in staticmethods and metaclasses should be rejected"
type: issue
state: open
author: stefanboca
labels:
  - typing semantics
assignees: []
created_at: 2025-12-15T18:16:39Z
updated_at: 2025-12-15T18:20:47Z
url: https://github.com/astral-sh/ty/issues/1897
synced_at: 2026-01-12T15:54:26Z
```

# `typing.Self` in staticmethods and metaclasses should be rejected

---

_@stefanboca_

According to the typing documentation, [`Self` in staticmethods and metaclasses is rejected](https://typing.python.org/en/latest/spec/generics.html#self:~:text=Note%20that%20we%20reject%20Self%20in%20staticmethods%2E%20Self).
```python
from typing import Self, Any

class Base:
    @staticmethod
    def make() -> Self:  # should be rejected
        ...

    @staticmethod
    def return_parameter(foo: Self) -> Self:  # should be rejected
        ...

class MyMetaclass(type):
    def __new__(cls, *args: Any) -> Self:  # should be rejected
        return super().__new__(cls, *args)

    def __mul__(cls, count: int) -> list[Self]:  # should be rejected
        return [cls()] * count

class Foo(metaclass=MyMetaclass): ...
```
https://play.ty.dev/017c6126-8d90-4619-a721-51eeff356725

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-15 18:20_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-15 18:20_

---
