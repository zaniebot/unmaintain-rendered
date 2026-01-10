```yaml
number: 871
title: "Prevent overriding of `Final` attributes in subclasses"
type: issue
state: open
author: sharkdp
labels:
  - typing semantics
assignees: []
created_at: 2025-07-22T14:27:33Z
updated_at: 2025-07-23T23:21:38Z
url: https://github.com/astral-sh/ty/issues/871
synced_at: 2026-01-10T02:06:24Z
```

# Prevent overriding of `Final` attributes in subclasses

---

_Issue opened by @sharkdp on 2025-07-22 14:27_

Attributes which are qualified as `Final` may not be overwritten in subclasses ([spec](https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final)):

```py
from typing import Final

class Base:
    CONSTANT: Final[int] = 1

class Derived(Base):
    CONSTANT = 2  # This should be an error
```

We have an existing test (with TODOs) for this [here](https://github.com/astral-sh/ruff/blob/64e57800373354cbabf106a5e89f6abc19d67300/crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md?plain=1#L230-L249).

I imagine that it might be convenient to implement this once we have https://github.com/astral-sh/ty/issues/166 (because that would require similar mechanisms to look up equivalent attributes in base classes), but it might also make sense to implement this independently.

---

_Label `typing semantics` added by @sharkdp on 2025-07-22 14:27_

---

_Added to milestone `GA` by @carljm on 2025-07-23 23:21_

---
