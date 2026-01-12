```yaml
number: 1985
title: Wrong function argument inlay hint for overloaded function
type: issue
state: open
author: benruijl
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-17T08:37:56Z
updated_at: 2025-12-31T15:47:21Z
url: https://github.com/astral-sh/ty/issues/1985
synced_at: 2026-01-12T15:54:26Z
```

# Wrong function argument inlay hint for overloaded function

---

_@benruijl_

### Summary

This small script gives a wrong type hint for the argument `x` and `y` which should be part of `*names` but are interpreted as the value for `name` and the `is_symmetric` argument of another overloaded variant:

```python
from typing import overload, Optional, Sequence

@overload
def S(name: str, is_symmetric: Optional[bool] = None) -> str:
    pass

@overload
def S(*names: str, is_symmetric: Optional[bool] = None) -> Sequence[str]:
    pass

def S():
    pass

b = S('x', 'y') # argument hints for the wrong overloaded variant, return type is correct
```

<img width="426" height="27" alt="Wrong type hint" src="https://github.com/user-attachments/assets/c45523c8-e389-484d-8bb3-41a15d72fc90" />

Here is the playground link:

https://play.ty.dev/0a5f6d13-1e2d-4fca-adef-80af38cf120f

### Version

_No response_

---

_Label `server` added by @sharkdp on 2025-12-17 08:41_

---

_Label `bug` added by @sharkdp on 2025-12-17 08:41_

---

_Comment by @sharkdp on 2025-12-17 08:42_

Thank you for reporting this!

---

_Renamed from "Wrong function argument hint for overloaded function" to "Wrong function argument inlay hint for overloaded function" by @AlexWaygood on 2025-12-17 08:44_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:47_

---
