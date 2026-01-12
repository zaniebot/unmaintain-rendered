```yaml
number: 1047
title: KW_ONLY should only apply to own-fields
type: issue
state: closed
author: carljm
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-08-19T00:43:42Z
updated_at: 2025-08-19T18:01:36Z
url: https://github.com/astral-sh/ty/issues/1047
synced_at: 2026-01-12T15:54:24Z
```

# KW_ONLY should only apply to own-fields

---

_@carljm_

In [this example](https://play.ty.dev/7bcf7ebb-e27e-4104-8f94-ecf2d10d1a43):

```py
from dataclasses import dataclass, KW_ONLY

@dataclass
class D:
    x: int
    _: KW_ONLY
    y: str

@dataclass
class E(D):
    z: bytes

E(1, b"foo", y="foo")
```

The field `z` should not be considered keyword-only; the `_: KW_ONLY` in `D` applies only to `y`, not to `z` in the subclass.

We currently handle `KW_ONLY` when we synthesize the `__init__` member; I think we maybe need to handle it in `own_fields` instead?

---

_Label `bug` added by @carljm on 2025-08-19 00:43_

---

_Label `dataclasses` added by @carljm on 2025-08-19 00:43_

---

_Closed by @carljm on 2025-08-19 18:01_

---
