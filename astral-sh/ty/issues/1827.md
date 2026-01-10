```yaml
number: 1827
title: "`typing.Self` appears to not work properly"
type: issue
state: closed
author: keysmashes
labels: []
assignees: []
created_at: 2025-12-09T17:03:44Z
updated_at: 2025-12-09T17:10:13Z
url: https://github.com/astral-sh/ty/issues/1827
synced_at: 2026-01-10T01:56:41Z
```

# `typing.Self` appears to not work properly

---

_Issue opened by @keysmashes on 2025-12-09 17:03_

### Summary

https://play.ty.dev/762f08f3-d8c9-4a52-954e-e4525b0e3062:

```python
#%%
from typing import Self

class C:
    next: Self | None
    name: str

    def __init__(self, name: str, next: Self | None):
        self.name = name
        self.next = next

    def get_next_name(self):
        return self.next.name if self.next is not None else None

print(C("foo", C("bar", None)).get_next_name())  # prints "bar" at runtime
```

ty complains about two errors:

```
    Object of type `Self@__init__ | None` is not assignable to attribute `next` of type `<special form 'typing.Self'> | None` (invalid-assignment) [Ln 10, Col 9]
    Special form `typing.Self` has no attribute `name` (unresolved-attribute) [Ln 13, Col 16]
```

Apologies if it's known that this isn't yet implemented!

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Comment by @AlexWaygood on 2025-12-09 17:10_

Thanks!

---

_Closed by @AlexWaygood on 2025-12-09 17:10_

---
