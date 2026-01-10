```yaml
number: 230
title: generic classes
type: issue
state: closed
author: carljm
labels:
  - generics
assignees: []
created_at: 2024-11-07T15:31:27Z
updated_at: 2025-05-29T23:40:35Z
url: https://github.com/astral-sh/ty/issues/230
synced_at: 2026-01-10T02:34:09Z
```

# generic classes

---

_Issue opened by @carljm on 2024-11-07 15:31_

_No description provided._

---

_Assigned to @carljm by @carljm on 2024-11-07 15:31_

---

_Comment by @carljm on 2024-11-07 15:33_

Probably depends on https://github.com/astral-sh/ruff/issues/14164 for meaningful support here

---

_Comment by @AlexWaygood on 2024-11-07 16:29_

As part of this, we should also fix the following bug in MRO inference:

```py
from typing import reveal_type

class Foo[T]: ...

reveal_type(Foo.__mro__)  # revealed: tuple[Literal[Foo], Literal[object]]
print(Foo.__mro__)  # prints `(<class '__main__.Foo'>, <class 'typing.Generic'>, <class 'object'>)`
```

---

_Unassigned @carljm by @carljm on 2025-01-10 15:51_

---

_Assigned to @dcreager by @carljm on 2025-03-06 18:05_

---

_Renamed from "[red-knot] generic classes" to "generic classes" by @MichaReiser on 2025-05-07 15:27_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 16:01_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 20:01_

---

_Removed from milestone `Alpha` by @MichaReiser on 2025-05-16 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-16 12:19_

---

_Comment by @carljm on 2025-05-29 23:40_

I think we can consider this done and use more specific issues for follow-ups and fixes. Congrats @dcreager !

---

_Closed by @carljm on 2025-05-29 23:40_

---
