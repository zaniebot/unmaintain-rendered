```yaml
number: 73
title: Update hover implementation for overloads to account for the matched signature
type: issue
state: closed
author: dhruvmanila
labels:
  - server
  - overloads
assignees: []
created_at: 2025-05-01T20:16:52Z
updated_at: 2025-11-20T17:45:04Z
url: https://github.com/astral-sh/ty/issues/73
synced_at: 2026-01-12T15:54:22Z
```

# Update hover implementation for overloads to account for the matched signature

---

_@dhruvmanila_

This is a nice to have feature which Pyright has.

Consider the following example:
```py
from typing import overload

@overload
def foo() -> None: ...
@overload
def foo(x: int) -> int: ...
def foo(x: int | None = None) -> int | None:
    return x


foo()
foo(2)
```

Asking for the hover content on the first `foo` call would give:
```py
(function) def foo() -> None
```

while on the second `foo` call would give:
```py
(function) def foo(x: int) -> int
```

---

_Label `server` added by @dhruvmanila on 2025-05-01 20:16_

---

_Renamed from "[red-knot] Update hover implementation for overloads to account for the matched signature" to "Update hover implementation for overloads to account for the matched signature" by @MichaReiser on 2025-05-07 15:12_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:04_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 07:05_

---

_Assigned to @Gankra by @Gankra on 2025-08-19 12:24_

---

_Comment by @MichaReiser on 2025-10-21 09:59_

Go to declaration should probably also jump to the implementation, whereas go to definition should jump to the matching overload

---

_Comment by @Gankra on 2025-10-23 20:58_

I think those are backwards but I otherwise agree.

---

_Closed by @Gankra on 2025-11-20 17:45_

---
