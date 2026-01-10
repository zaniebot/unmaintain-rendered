```yaml
number: 15780
title: UP035 recommends change that changes runtime behavior for is_typeddict
type: issue
state: closed
author: SebbyLaw
labels:
  - bug
assignees: []
created_at: 2025-01-28T05:40:18Z
updated_at: 2025-01-29T18:05:47Z
url: https://github.com/astral-sh/ruff/issues/15780
synced_at: 2026-01-10T11:09:57Z
```

# UP035 recommends change that changes runtime behavior for is_typeddict

---

_Issue opened by @SebbyLaw on 2025-01-28 05:40_

### Description

`is_typeddict` from the `typing` module does not correctly identify `TypedDict` from the `typing_extensions` module:
```py
from typing_extensions import is_typeddict as is_typeddict_te, TypedDict as TypedDict_te
from typing import is_typeddict, TypedDict

class Ext(TypedDict_te):
    a: int

class Reg(TypedDict):
    a: int

print("is_typeddict(Ext):", is_typeddict(Ext))  # False
print("is_typeddict(Reg):", is_typeddict(Reg))  # True
print("is_typeddict_te(Ext):", is_typeddict_te(Ext))  # True
print("is_typeddict_te(Reg):", is_typeddict_te(Reg))  # True
```
In the description of UP035:
> Note that, in some cases, it may be preferable to continue importing members from `typing_extensions` even after they're added to the Python standard library, as `typing_extensions` can backport bugfixes and optimizations from later Python versions. This rule thus avoids flagging imports from `typing_extensions` in such cases.



---

_Label `bug` added by @AlexWaygood on 2025-01-28 11:29_

---

_Closed by @AlexWaygood on 2025-01-29 18:05_

---
