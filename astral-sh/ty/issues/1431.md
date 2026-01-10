```yaml
number: 1431
title: Consider enabling goto definition for augmented-assignment operators
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2025-10-24T09:35:52Z
updated_at: 2025-11-18T16:10:40Z
url: https://github.com/astral-sh/ty/issues/1431
synced_at: 2026-01-10T01:58:59Z
```

# Consider enabling goto definition for augmented-assignment operators

---

_Issue opened by @AlexWaygood on 2025-10-24 09:35_

Previously discussed in https://github.com/astral-sh/ty/issues/1151#issuecomment-3430803296

In this snippet, we currently allow you to jump from the `+` operator to `Foo.__add__`, but we do not jump from the `+=` operator to `Bar.__iadd__`. For consistency, I would intuitively expect `+=` to work for `__iadd__` here exactly the same as `+` works for `__add__`: they're both operators in Python, and they both have well-defined semantics at runtime:

```py
class Foo:
    def __add__(self, other): ...

Foo() + Foo()

class Bar:
    def __iadd__(self, other): ...

a = Bar()
a += Bar()
```

Pylance does not support this, but I'm not sure why that is; it may just be an oversight?

---

_Label `server` added by @AlexWaygood on 2025-10-24 09:35_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:27_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
