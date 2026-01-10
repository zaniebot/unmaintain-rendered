```yaml
number: 1241
title: "Incorrect inferences for `.value` property for enum members that use `enum.auto()`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - runtime semantics
  - enums
assignees: []
created_at: 2025-09-23T15:42:26Z
updated_at: 2025-11-10T17:53:10Z
url: https://github.com/astral-sh/ty/issues/1241
synced_at: 2026-01-10T02:06:25Z
```

# Incorrect inferences for `.value` property for enum members that use `enum.auto()`

---

_Issue opened by @AlexWaygood on 2025-09-23 15:42_

### Summary

Ty still has incorrect inferences for the `.value` property for enum members that use `enum.auto()` in at least two situations:
- Enums that use custom (non-integer) mixin types: for these enums, ty incorrectly infers that their values are all integers, when this is not the case
- Enums that have only a single member: for these enums, ty appears to infer the `.value` property as resolving to `Any` (if there is no mixin, or the mixin is `int`, it can safely infer a `Literal`-integer type for these, the same as if there are multiple enum members)

```py
import enum

class A(str, enum.Enum):
    X = enum.auto()
    Y = enum.auto()

reveal_type(A.X.value)  # ty: Literal[1]; runtime value: `"1"`. We should probably either infer `str` or `Any`?

class B(bytes, enum.Enum):
    X = enum.auto()
    Y = enum.auto()

reveal_type(B.X.value)  # ty: Literal[1]; runtime value: `b'\x00'`. We should probably either infer `bytes` or `Any`?

class C(enum.Enum):
    X = enum.auto()

reveal_type(C.X.value)  # ty: Any; runtime value: `1`. We should probably infer `Literal[1]`?
```

https://play.ty.dev/6683ffc4-54d5-4fc0-b5d7-e4c05f023965

@thejchap, you might be interested in taking this on! No pressure if you don't feel like it though :-)

---

_Label `bug` added by @AlexWaygood on 2025-09-23 15:42_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-09-23 15:42_

---

_Label `enums` added by @AlexWaygood on 2025-09-23 19:59_

---

_Closed by @AlexWaygood on 2025-11-10 17:53_

---
