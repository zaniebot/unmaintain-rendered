```yaml
number: 2566
title: "Assignment in PySide6 slots causes fallback to `Unknown`"
type: issue
state: open
author: philj56
labels: []
assignees: []
created_at: 2026-01-19T17:03:34Z
updated_at: 2026-01-19T17:03:34Z
url: https://github.com/astral-sh/ty/issues/2566
synced_at: 2026-01-19T17:25:58Z
```

# Assignment in PySide6 slots causes fallback to `Unknown`

---

_@philj56_

### Summary

I'm not sure if this counts as a bug in `ty`, but if a member variable with an explicit type hint is assigned to in a method wrapped in PySide's `@Slot()` decorator, `ty` falls back to typing the variable as `Unknown`, unless the slot also contains a type hint.

Example:
```python
from typing import reveal_type

from PySide6.QtCore import QObject, Slot


class WithoutSlot(QObject):
    def __init__(self):
        self.x: int = 3

    def set_x(self):
        self.x = 4

class WithSlot(QObject):
    def __init__(self):
        self.x: int = 3

    @Slot()
    def set_x(self):
        self.x = 4

class WithSlotAndAnnotation(QObject):
    def __init__(self):
        self.x: int = 3

    @Slot()
    def set_x(self):
        self.x: int = 4

reveal_type(WithoutSlot().x)
reveal_type(WithSlot().x)
reveal_type(WithSlotAndAnnotation().x)
```

Output:
```
$ ty check .\ty_slot.py
info[revealed-type]: Revealed type
  --> ty_slot.py:29:13
   |
27 |         self.x: int = 4
28 |
29 | reveal_type(WithoutSlot().x)
   |             ^^^^^^^^^^^^^^^ `int`
30 | reveal_type(WithSlot().x)
31 | reveal_type(WithSlotAndAnnotation().x)
   |

info[revealed-type]: Revealed type
  --> ty_slot.py:30:13
   |
29 | reveal_type(WithoutSlot().x)
30 | reveal_type(WithSlot().x)
   |             ^^^^^^^^^^^^ `Unknown | int`
31 | reveal_type(WithSlotAndAnnotation().x)
   |

info[revealed-type]: Revealed type
  --> ty_slot.py:31:13
   |
29 | reveal_type(WithoutSlot().x)
30 | reveal_type(WithSlot().x)
31 | reveal_type(WithSlotAndAnnotation().x)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^ `int`
   |

Found 3 diagnostics
```

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---
