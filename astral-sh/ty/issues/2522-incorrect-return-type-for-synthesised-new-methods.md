```yaml
number: 2522
title: "Incorrect return type for synthesised `__new__` methods of `NamedTuple` classes"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - typing semantics
  - namedtuples
assignees: []
created_at: 2026-01-15T20:44:43Z
updated_at: 2026-01-16T00:17:17Z
url: https://github.com/astral-sh/ty/issues/2522
synced_at: 2026-01-16T01:03:50Z
```

# Incorrect return type for synthesised `__new__` methods of `NamedTuple` classes

---

_@AlexWaygood_

It looks like the `__new__` method we synthesize for `NamedTuple` classes is buggy -- here's some minimal repros of some diagnostics in the ecosystem report on this PR:

```py
from collections import namedtuple
from typing import NamedTuple, Self

class Foo(NamedTuple):
    x: int

class Bar(Foo):
    def __new__(cls) -> "Bar":
        return super().__new__(cls, x=42)  # error[invalid-return-type] "Return type does not match returned value: expected `Bar`, found `Foo`"

class Bar2(Foo):
    def __new__(cls) -> Self:
        return super().__new__(cls, x=42)  # error[invalid-return-type] "Return type does not match returned value: expected `Self@__new__`, found `Foo`"

Baz = namedtuple("Baz", "x y")

class Spam(Baz):
    def __new__(cls) -> "Spam":
        return super().__new__(cls, x=42, y=56)  # error[invalid-return-type] "Return type does not match returned value: expected `Spam`, found `Baz`"

class Spam2(Baz):
    def __new__(cls) -> Self:
        return super().__new__(cls, x=42, y=56)  # error[invalid-return-type] "Return type does not match returned value: expected `Self@__new__`, found `Baz`"
```

I think the return type we synthesize for these methods changed in https://github.com/astral-sh/ruff/commit/3e0299488e72fa80f59473565cf724041d0f3f3c. We need to infer these methods as returning `Self` rather than an instance of the namedtuple class, I think. cc. @charliermarsh

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/issues/22584#issuecomment-3755022956_
            

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-15 20:44_

---

_Label `bug` added by @AlexWaygood on 2026-01-15 20:45_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-15 20:45_

---

_Added to milestone `Stable` by @carljm on 2026-01-16 00:17_

---
