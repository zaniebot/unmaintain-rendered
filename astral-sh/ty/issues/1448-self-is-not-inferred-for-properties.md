```yaml
number: 1448
title: Self is not inferred for properties
type: issue
state: closed
author: vlashada
labels:
  - bug
  - attribute access
assignees: []
created_at: 2025-10-27T21:46:55Z
updated_at: 2025-10-29T21:22:40Z
url: https://github.com/astral-sh/ty/issues/1448
synced_at: 2026-01-12T15:54:25Z
```

# Self is not inferred for properties

---

_@vlashada_

### Summary

```python
from typing import reveal_type

class A:
    name: str
    def b(self):
        reveal_type(self.name) # type: str
    @property
    def c(self):
        reveal_type(self.name) # type: unknown
```

https://play.ty.dev/1a887aa0-89b8-464f-94f6-691182a64de5

### Version

_No response_

---

_Label `bug` added by @sharkdp on 2025-10-28 08:33_

---

_Label `attribute access` added by @sharkdp on 2025-10-28 08:33_

---

_Comment by @sharkdp on 2025-10-28 08:33_

Thank you for reporting this.

---

_Assigned to @sharkdp by @sharkdp on 2025-10-29 11:51_

---

_Comment by @sharkdp on 2025-10-29 12:19_

A similar problem exists for *all* decorated methods, and all methods whose function definition is later overwritten.


The problem is that [this check](https://github.com/astral-sh/ruff/blob/349061117c18683db607bc66140d5c870210b40a/crates/ty_python_semantic/src/types/infer/builder.rs#L2596-L2599) is too naive. It picks up the type of the symbol at the end of the class scope, so in the OP case, a `property` object, not a function.

---

_Closed by @sharkdp on 2025-10-29 21:22_

---
