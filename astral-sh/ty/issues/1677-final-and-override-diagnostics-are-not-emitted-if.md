```yaml
number: 1677
title: "`@final` and `@override` diagnostics are not emitted if there are multiple reachable definitions"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2025-11-29T15:51:25Z
updated_at: 2025-12-03T12:51:37Z
url: https://github.com/astral-sh/ty/issues/1677
synced_at: 2026-01-12T15:54:25Z
```

# `@final` and `@override` diagnostics are not emitted if there are multiple reachable definitions

---

_@AlexWaygood_

### Summary

Consider this snippet:

```py
from typing import final, override

def coinflip() -> bool:
    return False

class A:
    @final
    def method(self) -> None: ...

    if coinflip():
        @override
        def wut(self) -> None: ...
    else:
        @override
        def wut(self) -> None: ...

class B(A):
    if coinflip():
        def method(self) -> None: ...
    else:
        def method(self) -> None: ...
```

We should emit diagnostics on the definitions of `A.wut` and `B.method`, but we currently do not:
- `A.wut` is marked with `@override` but doesn't override anything
- `B.method` overrides a method decorated with `@final`

### Version

_No response_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-29 15:51_

---

_Label `bug` added by @AlexWaygood on 2025-11-29 15:51_

---

_Comment by @AlexWaygood on 2025-11-29 16:12_

Note that we do emit these diagnostics if there is a single possibly-undefined definition:

```py
from typing import final, override

def coinflip() -> bool:
    return False

class A:
    @final
    def method(self) -> None: ...

    if coinflip():
        @override
        def wut(self) -> None: ...  # diagnostic correctly emitted here

class B(A):
    if coinflip():
        def method(self) -> None: ...  # diagnostic correctly emitted here
```

---

_Closed by @AlexWaygood on 2025-12-03 12:51_

---
