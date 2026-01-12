```yaml
number: 1681
title: "Add diagnostic warning about unsound subclassing of `order=True` dataclasses"
type: issue
state: open
author: AlexWaygood
labels:
  - dataclasses
  - runtime semantics
assignees: []
created_at: 2025-11-29T16:38:07Z
updated_at: 2025-11-30T15:46:08Z
url: https://github.com/astral-sh/ty/issues/1681
synced_at: 2026-01-12T15:54:25Z
```

# Add diagnostic warning about unsound subclassing of `order=True` dataclasses

---

_@AlexWaygood_

It's unsound to subclass an `order=True` dataclass, because the generated comparison methods raise an exception if you try to compare an instance of the subclass with an instance of the superclass:

```py
from dataclasses import dataclass

@dataclass(order=True)
class F:
    x: int

class G(F): ...

G(42) < F(42)  # exception raised here at runtime, but cannot detected by ty
```

We should detect attempts to subclass `order=True` dataclasses and warn users about the unsoundness (emitting a diagnostic at the point where `G` is defined). We could recommend that they use `functools.total_ordering` instead to generate their comparison methods, as `total_ordering` doesn't have the same problem.

(One way of describing the problem here is that the design of the stdlib feature violates the Liskov Substitution Principle.)

---

_Label `runtime semantics` added by @AlexWaygood on 2025-11-29 16:38_

---

_Label `dataclasses` added by @AlexWaygood on 2025-11-29 18:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-30 15:46_

---
