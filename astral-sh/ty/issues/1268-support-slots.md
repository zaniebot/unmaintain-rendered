```yaml
number: 1268
title: "support `__slots__`"
type: issue
state: open
author: mtshiba
labels:
  - runtime semantics
assignees: []
created_at: 2025-09-27T08:01:19Z
updated_at: 2025-11-12T23:44:46Z
url: https://github.com/astral-sh/ty/issues/1268
synced_at: 2026-01-12T15:54:24Z
```

# support `__slots__`

---

_@mtshiba_

`__slots__` is already handled specially in ty, but should be more fully supported in line with the Python specification.
`__slots__` may not be a very commonly used feature, but several major libraries use it (e.g. [attrs](https://github.com/python-attrs/attrs/blob/50334f3fe759013f52cc533161e9b474e98bc460/src/attr/_make.py#L2436)), and supporting it could improve inference results and error reporting.

ref: https://docs.python.org/3/reference/datamodel.html#slots

Here are the features that should be implemented:

- [ ] Assume that the attributes defined in `__slots__` are not unresolved
- [ ] Attempting to read/write/declare an attribute that is not defined in `__slots__` will be reported as an error
- [ ] Defining a class variable to set a default value for a name that is defined in `__slots__` will be reported as an error
- [ ] Remove `__dict__` and `__weakref__` from classes that define `__slots__`
- [ ] An error will be raised if nonempty `__slots__` are defined for a class derived from a "variable-length" built-in type such as int, bytes, and tuple

```python
class C:
    __slots__ = ("foo",)

    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar  # error

    baz: int  # error (pyright doesn't report this as an error)
    foo: int = 1  # error

class D:
    __slots__ = ("bar",)

d = D()
reveal_type(d.bar)  # revealed: Any or Unknown
reveal_type(d.foo)  # error
d.bar = 1  # ok
# Classes with `__slots__` defined will not have a `__dict__` by default.
d.foo = 1  # error

class E:
    __slots__ = ("foo", "__dict__")

    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar  # ok

# `F` has `__dict__` and `__weakref__`.
class F: ...

# Even if a class defines `__slots__`, if the superclass has `__dict__` or `__weakref__`,
# those attributes will be inherited.
class G(F):
    __slots__ = ("foo",)

    def __init__(self, foo, bar):
        self.foo = foo
        self.bar = bar  # ok

# error: nonempty __slots__ not supported for subtype of 'int'
class H(int):
    __slots__ = ("foo",)
```

related: #111

---

_Label `runtime semantics` added by @sharkdp on 2025-09-29 07:27_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-10-17 14:41_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-17 14:42_

---

_Removed from milestone `Beta` by @carljm on 2025-11-12 23:44_

---

_Added to milestone `Stable` by @carljm on 2025-11-12 23:44_

---
