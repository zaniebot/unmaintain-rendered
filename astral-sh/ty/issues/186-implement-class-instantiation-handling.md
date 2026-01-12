```yaml
number: 186
title: Implement class instantiation handling
type: issue
state: closed
author: mishamsk
labels:
  - calls
assignees: []
created_at: 2025-03-05T03:14:10Z
updated_at: 2025-06-10T23:54:25Z
url: https://github.com/astral-sh/ty/issues/186
synced_at: 2026-01-12T15:54:22Z
```

# Implement class instantiation handling

---

_@mishamsk_

## Description

Currently red knot assumes all calls to class literals are valid and create instances. E.g. [this test](https://github.com/astral-sh/ruff/blob/bb44926ca5555b3dce59d1c14d5dbecee3cf9825/crates/red_knot_python_semantic/resources/mdtest/unary/invert_add_usub.md?plain=1#L8C4-L21C13) yields no diagnostic currently

```py
from typing import Literal

class Number:
    def __init__(self, value: int):
        self.value = 1

    def __pos__(self) -> int:
        return +self.value

    def __neg__(self) -> int:
        return -self.value

    def __invert__(self) -> Literal[True]:
        return True

a = Number()
```

We should check `__init__` signature just like any other call and emit diagnostics.

This is step 1 in a series of feature to support the instance creation process in full. The related logic of handling `type[C]` is tracked in astral-sh/ruff#15948 

---

_Renamed from "[red-knot] Check __init__ arguments on class literals call" to "[red-knot] Check __init__ arguments when doing `try_call` on a class literal" by @mishamsk on 2025-03-05 03:16_

---

_Renamed from "[red-knot] Check __init__ arguments when doing `try_call` on a class literal" to "[red-knot] Check `__init__` arguments when doing `try_call` on a class literal" by @mishamsk on 2025-03-05 03:16_

---

_Assigned to @mishamsk by @carljm on 2025-03-06 18:12_

---

_Renamed from "[red-knot] Check `__init__` arguments when doing `try_call` on a class literal" to "[red-knot] Implement class instantiation handling" by @mishamsk on 2025-04-09 11:30_

---

_Renamed from "[red-knot] Implement class instantiation handling" to "Implement class instantiation handling" by @MichaReiser on 2025-05-07 15:26_

---

_Label `calls` added by @AlexWaygood on 2025-05-11 10:53_

---

_Comment by @carljm on 2025-06-10 23:54_

I think this is done, apart from https://github.com/astral-sh/ty/issues/281 which has its own issue?

---

_Closed by @carljm on 2025-06-10 23:54_

---
