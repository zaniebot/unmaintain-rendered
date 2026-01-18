```yaml
number: 2515
title: "Emit a diagnostic on `NamedTuple` classes decorated with `@dataclass`"
type: issue
state: closed
author: AlexWaygood
labels:
  - dataclasses
  - runtime semantics
  - typing semantics
  - namedtuples
assignees: []
created_at: 2026-01-15T12:40:59Z
updated_at: 2026-01-18T17:20:11Z
url: https://github.com/astral-sh/ty/issues/2515
synced_at: 2026-01-18T18:15:23Z
```

# Emit a diagnostic on `NamedTuple` classes decorated with `@dataclass`

---

_@AlexWaygood_

If a user does this, it indicates that they're confused, because attempting to create an instance of a class like this always fails at runtime:

```py
from dataclasses import dataclass
from typing import NamedTuple

@dataclass
class Foo(NamedTuple):
    x: int
    y: str

# Both of these calls fail with `AttributeError: can't set attribute`
Foo(42, "y")
Foo(x=42, y="y")
```

We don't model the semantics here correctly right now (we don't synthesize any of the `NamedTuple` generated methods on this class at all -- we think, for instance, that this class inherits its `__new__` method from `tuple`). But we don't need to model the semantics correctly if we detect that this is an error (which it is) and emit an appropriate diagnostic.

We added a TODO for this in the code in https://github.com/astral-sh/ruff/commit/3e0299488e72fa80f59473565cf724041d0f3f3c, but I don't think we added any tests for it.

---

_Assigned to @charliermarsh by @AlexWaygood on 2026-01-15 12:40_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-15 12:41_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-15 12:41_

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-15 12:41_

---

_Label `dataclasses` added by @AlexWaygood on 2026-01-15 16:41_

---

_Closed by @charliermarsh on 2026-01-18 17:20_

---
