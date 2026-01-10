```yaml
number: 1427
title: "alpha.24 regression: Non-dataclass inheriting from a dataclass breaks generic inference"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - generics
  - dataclasses
assignees: []
created_at: 2025-10-24T00:23:44Z
updated_at: 2025-10-31T12:55:20Z
url: https://github.com/astral-sh/ty/issues/1427
synced_at: 2026-01-10T02:06:25Z
```

# alpha.24 regression: Non-dataclass inheriting from a dataclass breaks generic inference

---

_Issue opened by @MeGaGiGaGon on 2025-10-24 00:23_

### Summary

This is a MRE of some code that stopped working when I tried `ty 0.0.1-alpha.24` [playground](https://play.ty.dev/d62bf40e-aa4a-432e-9e9a-2c4ab01d9c3c). If you add `@dataclass` to `ChildOfParentDataclass` the error goes away.

Edit: On playing around more, this looks to be an issue with/regression in the generics inference. Making the generic explicit with `return ChildOfParentDataclass[T](x)` also makes the error go away.

```py
# Gives incorrect error when using a dataclass parent:

from dataclasses import dataclass

@dataclass
class ParentDataclass[T]:
    value: T

# @dataclass  # Uncommenting this makes the error go away
class ChildOfParentDataclass[T](ParentDataclass[T]): ...

def uses_dataclass[T](x: T) -> ChildOfParentDataclass[T]:
    # Argument is incorrect: Expected `T@ChildOfParentDataclass`, found `T@uses_dataclass`
    return ChildOfParentDataclass(x)

# Works fine if the parent is a normal class

class Parent[T]:
    def __init__(self, value: T):
        self.value = value

class ChildOfParent[T](Parent[T]): ...

def uses_init[T](x: T) -> ChildOfParent[T]:
    return ChildOfParent(x)
```
Comparing alpha.23 to alpha.24:
```powershell
PS ~>uvx ty@0.0.1-alpha.23 check issue.py
Checking ------------------------------------------------------------ 1/1 files
All checks passed!
PS ~>uvx ty@0.0.1-alpha.24 check issue.py
Checking ------------------------------------------------------------ 1/1 files
error[invalid-argument-type]: Argument is incorrect
  --> issue.py:14:35
   |
12 | def uses_dataclass[T](x: T) -> ChildOfParentDataclass[T]:
13 |     # Argument is incorrect: Expected T@ChildOfParentDataclass, found T@uses_dataclass
14 |     return ChildOfParentDataclass(x)
   |                                   ^ Expected `T@ChildOfParentDataclass`, found `T@uses_dataclass`
15 |
16 | # Works fine if the parent is a normal class
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.24 (1fee7da8b 2025-10-23)

---

_Renamed from "alpha.24 regression: Non-dataclass inheriting from a dataclass breaks generics" to "alpha.24 regression: Non-dataclass inheriting from a dataclass breaks generic inference" by @MeGaGiGaGon on 2025-10-24 03:57_

---

_Label `bug` added by @AlexWaygood on 2025-10-24 09:37_

---

_Label `generics` added by @AlexWaygood on 2025-10-24 09:37_

---

_Label `dataclasses` added by @AlexWaygood on 2025-10-24 09:37_

---

_Comment by @sharkdp on 2025-10-27 12:31_

Thank you for reporting this. Something seems broken with the `__init__` methods of generic dataclasses:

```py
from dataclasses import dataclass

@dataclass
class ParentDataclass[T]:
    value: T

class ChildOfParentDataclass[T](ParentDataclass[T]): ...

# (self: ParentDataclass[Unknown], value: Unknown) -> None
reveal_type(ParentDataclass.__init__)

# (self: ParentDataclass[T@ChildOfParentDataclass], value: T@ChildOfParentDataclass) -> None
reveal_type(ChildOfParentDataclass.__init__)
```

---

_Added to milestone `Beta` by @carljm on 2025-10-30 16:03_

---

_Comment by @saada on 2025-10-31 03:26_

I've submitted a PR to fix this issue: https://github.com/astral-sh/ruff/pull/21159

The fix threads the child class's generic context through to the parent's synthesized `__init__` method, restoring the behavior from alpha.23. All tests passing and verified against mypy and pyright.

---

_Closed by @sharkdp on 2025-10-31 12:55_

---
