---
number: 1138
title: "Detect class attributes set by metaclasses `__init__` method"
type: issue
state: open
author: strangemonad
labels:
  - attribute access
assignees: []
created_at: 2025-09-06T01:21:25Z
updated_at: 2026-01-09T02:40:44Z
url: https://github.com/astral-sh/ty/issues/1138
synced_at: 2026-01-10T01:48:23Z
---

# Detect class attributes set by metaclasses `__init__` method

---

_Issue opened by @strangemonad on 2025-09-06 01:21_

### Summary

I'm not sure if this is a patito-specific issue or a more general metaclass lookup issue.

https://play.ty.dev/2e6bfe16-f59c-46f0-82c0-aaebcacb942d

Context
- polars is a popular dataframe library
- patito allows you to define the schema of individual rows as pydantic models
- This code checks in pyright/basepyright. I haven't checked mypy, pyre or others.

The implementation relies on a metaclass to define some of this behavior.
 
In particular in the following code
```python
import patito as pt

class Person(pt.Model):
    name: str

results = Person.DataFrame(...)
```

The `Person.DataFrame` class var is added by the `patito.pydantic.ModelMetaclass(PydanticModelMetaclass)`. It's a common and convenient shortcut for the getting a dataframe constructor that's already bound to the correct generic type var and run time model class to allow for runtime schema validation. The alternative would be something like the following.

```
results = pt.DataFrame[Person]().set_model(Person)
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Label `attribute access` added by @sharkdp on 2025-09-08 07:17_

---

_Comment by @sharkdp on 2025-09-08 07:19_

Thank you for reporting this. I agree that we should support this.

A self-contained example would be the following (https://play.ty.dev/d9452eef-2aa2-475e-a887-48fd63896a23):
```py
class Meta(type):
    def __init__(cls, name, bases, namespace, **kwargs) -> None:
        cls.DataFrame = 1

class C(metaclass=Meta):
    pass

reveal_type(C.DataFrame)
```

---

_Renamed from "Patito metaclass classvar is reported as error[unresolved-attribute]" to "Detect class attributes set by metaclasses `__init__` method" by @sharkdp on 2025-09-08 07:20_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 02:40_

---
