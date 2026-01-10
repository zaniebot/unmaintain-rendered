---
number: 2161
title: "Support dict literals and `dict()` calls as default values for parameters annotated with `TypedDict` types"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-12-22T15:23:59Z
updated_at: 2025-12-22T16:37:36Z
url: https://github.com/astral-sh/ty/issues/2161
synced_at: 2026-01-10T01:52:52Z
---

# Support dict literals and `dict()` calls as default values for parameters annotated with `TypedDict` types

---

_Issue opened by @AlexWaygood on 2025-12-22 15:23_

### Summary

Originally reported by @mflova in https://github.com/astral-sh/ty/issues/2127#issuecomment-3678156698. We should be able to use the type context of the parameter annotation to avoid emitting a diagnostic on either of these:

```py
from typing import TypedDict

class Foo(TypedDict):
    x: int

def x(a: Foo = {"x": 42}): ...
def y(a: Foo = dict(x=42)): ...
```


### Version

_No response_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-12-22 15:24_

---

_Label `bug` added by @AlexWaygood on 2025-12-22 15:24_

---

_Added to milestone `Stable` by @carljm on 2025-12-22 16:37_

---
