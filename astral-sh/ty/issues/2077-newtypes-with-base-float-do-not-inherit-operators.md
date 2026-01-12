```yaml
number: 2077
title: NewTypes with base float do not inherit operators correctly
type: issue
state: closed
author: ncollins-vs
labels:
  - bug
assignees: []
created_at: 2025-12-18T16:00:45Z
updated_at: 2026-01-06T17:32:24Z
url: https://github.com/astral-sh/ty/issues/2077
synced_at: 2026-01-12T15:54:26Z
```

# NewTypes with base float do not inherit operators correctly

---

_@ncollins-vs_

### Summary

Ty does not support many operators on `NewType`s with base `float`:

```python
from typing import NewType

Test = NewType("Test", float)
x = Test(5)
y = Test(10)
print(x > y) #  Unsupported `>` operation
```

This same snippet works if the base type is `int`

Playground: https://play.ty.dev/dee694b0-c39c-4402-afd0-6bdb2b24dbaa

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Renamed from "NewTypes with base float do not inherit operators correctl" to "NewTypes with base float do not inherit operators correctly" by @AlexWaygood on 2025-12-18 16:02_

---

_Comment by @AlexWaygood on 2025-12-18 19:21_

@oconnor663, would you be able to take a look at this one?

---

_Label `bug` added by @AlexWaygood on 2025-12-18 19:22_

---

_Added to milestone `Stable` by @carljm on 2025-12-18 19:24_

---

_Assigned to @oconnor663 by @carljm on 2025-12-18 19:24_

---

_Closed by @oconnor663 on 2026-01-06 17:32_

---
