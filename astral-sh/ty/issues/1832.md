```yaml
number: 1832
title: unspecialized generic ClassLiteral should be assignable to default-specialized GenericAlias
type: issue
state: closed
author: carljm
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-12-10T01:46:59Z
updated_at: 2025-12-10T16:18:10Z
url: https://github.com/astral-sh/ty/issues/1832
synced_at: 2026-01-10T01:56:41Z
```

# unspecialized generic ClassLiteral should be assignable to default-specialized GenericAlias

---

_Issue opened by @carljm on 2025-12-10 01:46_

With a final generic class `P`, we don't currently allow the class `P` itself to be assignable to the type `type[P]`:

```py
from typing import final 

@final
class P[T]:
    x: T

def expects_type_p(x: type[P]):
    pass

expects_type_p(P)  # this call currently fails but should succeed
```

Without the `@final`, this works, because [we do implement this assignability to subclass-of types](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L2696-L2725). But for a final class we simplify the subclass-of type to a simple GenericAlias type, and we don't implement assignability of an unspecialized ClassLiteral to a default-specialized GenericAlias type.

---

_Added to milestone `Beta` by @carljm on 2025-12-10 01:47_

---

_Label `bug` added by @carljm on 2025-12-10 01:47_

---

_Label `generics` added by @carljm on 2025-12-10 01:47_

---

_Label `type properties` added by @carljm on 2025-12-10 01:47_

---

_Closed by @sharkdp on 2025-12-10 16:18_

---
