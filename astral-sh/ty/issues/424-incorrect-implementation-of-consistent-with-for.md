```yaml
number: 424
title: Incorrect implementation of consistent-with for unions
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - generics
  - type properties
  - set-theoretic types
assignees: []
created_at: 2025-05-16T14:47:07Z
updated_at: 2025-05-16T17:37:08Z
url: https://github.com/astral-sh/ty/issues/424
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect implementation of consistent-with for unions

---

_@JelleZijlstra_

### Summary

```python
from typing import Any

def f(x: list[int]):
    g(x)
    
def g(x: list[bool | Any]):
    f(x)
```

ty reports errors on both of these (playground as of today):

```
    Argument to function `g` is incorrect: Expected `list[bool | Any]`, found `list[int]` (invalid-argument-type) [Ln 4, Col 7]
    Argument to function `f` is incorrect: Expected `list[int]`, found `list[bool | Any]` (invalid-argument-type) [Ln 7, Col 7]
```

I think this is wrong and both of these calls should be accepted:

- Invariant types are assignable if the type parameters are consistent with each other. (We don't say this explicitly in the spec, but I think that's how invariance should work.)
- Two gradual types are consistent with each other if there is some materialization that makes them equivalent.
- In `Any | bool`, we could materialize `Any` to `int`, which reduces to `int | bool` -> `int` (as `bool` is a subtype of `int`).
- Therefore, `Any | bool` and `int` are consistent.

Pyright also gets this wrong, though only in one direction (it accepts the `g(x)` call but not the `f(x)` call). Mypy gets it right.

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-05-16 14:50_

---

_Label `type properties` added by @AlexWaygood on 2025-05-16 14:50_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-16 14:50_

---

_Comment by @JelleZijlstra on 2025-05-16 14:55_

Another example where ty incorrectly reports an error in both directions:

```python
from typing import Any

def f(x: list[list[Any]]):
    g(x)
    
def g(x: list[list[int | str]]):
    f(x)
```

---

_Comment by @JelleZijlstra on 2025-05-16 15:01_

And another one where ty incorrectly reports two errors:

```python
from typing import Any

def f(x: list[tuple[Any] | tuple[bool]]):
    g(x)

def g(x: list[tuple[int]]):
    f(x)
```

(This is a counterexample to an implementation technique I was considering, where I'd special-case unions containing Any.)

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-16 16:48_

---

_Label `bug` added by @AlexWaygood on 2025-05-16 16:48_

---

_Closed by @AlexWaygood on 2025-05-16 17:37_

---
