```yaml
number: 347
title: "False positive: \"possibly-unbound-attribute\" when accessing attribute after if obj is not None guard"
type: issue
state: closed
author: romainbourdain
labels: []
assignees: []
created_at: 2025-05-13T08:26:44Z
updated_at: 2025-05-13T08:30:00Z
url: https://github.com/astral-sh/ty/issues/347
synced_at: 2026-01-12T15:54:23Z
```

# False positive: "possibly-unbound-attribute" when accessing attribute after if obj is not None guard

---

_@romainbourdain_

### Summary

Hi,

I'm encountering what seems to be a false positive from Ty when accessing an attribute on an optional object that has just been checked with if.

## Reproductible example
```python
from typing import Optional

class Node:
    def __init__(self, name: str, parent: Optional["Node"] = None):
        self.name = name
        self.parent = parent

positions = [("root", (0, 0)), ("child", (1, 1))]

node = Node("child", parent=Node("root"))

if node.parent:
    pos = next(p for name, p in positions if name == node.parent.name)[1]
```

## Ty output
```
Attribute `name` on type `Unknown | Node | None` is possibly unboundty(possibly-unbound-attribute)
```

### Version

_No response_

---

_Comment by @sharkdp on 2025-05-13 08:29_

Thank you for reporting this. We currently don't support narrowing on attribute expressions, yet. See #164.

---

_Closed by @sharkdp on 2025-05-13 08:29_

---
