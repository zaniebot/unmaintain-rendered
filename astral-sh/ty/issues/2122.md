```yaml
number: 2122
title: "Semantics of `typing.Self` not understood in child classes"
type: issue
state: closed
author: bepri
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-12-19T21:41:31Z
updated_at: 2026-01-08T00:56:11Z
url: https://github.com/astral-sh/ty/issues/2122
synced_at: 2026-01-10T01:56:41Z
```

# Semantics of `typing.Self` not understood in child classes

---

_Issue opened by @bepri on 2025-12-19 21:41_

### Summary

When attempting to override a method of a parent class with `-> Self` in a child class, calls to the parent's method are considered invalid.

```py
from typing_extensions import Self, override


class Parent:
    @classmethod
    def new(cls) -> Self:
        return cls()

class Child(Parent):
    @classmethod
    @override
    def new(cls) -> Self:
        return super().new()
```

The following code reports:

```
❯ ty check foo.py
error[invalid-return-type]: Return type does not match returned value
  --> foo.py:12:21
   |
10 |     @classmethod
11 |     @override
12 |     def new(cls) -> Self:
   |                     ---- Expected `Self@new` because of return type
13 |         return super().new()
   |                ^^^^^^^^^^^^^ expected `Self@new`, found `Child`
   |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

Running the same file through pyright and mypy yields no errors.

### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Label `bug` added by @AlexWaygood on 2025-12-19 21:43_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-19 21:43_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-19 21:43_

---

_Removed from milestone `Stable` by @carljm on 2025-12-20 16:56_

---

_Added to milestone `M1` by @carljm on 2025-12-20 16:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-25 12:04_

---

_Closed by @charliermarsh on 2026-01-08 00:56_

---
