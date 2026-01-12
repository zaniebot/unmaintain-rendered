```yaml
number: 621
title: "`invalid-return-type` false positive on constrained generic when narrowing for each constraint"
type: issue
state: open
author: DetachHead
labels:
  - narrowing
  - generics
assignees: []
created_at: 2025-06-10T10:59:08Z
updated_at: 2025-09-05T17:21:44Z
url: https://github.com/astral-sh/ty/issues/621
synced_at: 2026-01-12T15:54:23Z
```

# `invalid-return-type` false positive on constrained generic when narrowing for each constraint

---

_@DetachHead_

### Summary

```py
def foo[T: (int, str)](value: T) -> T:
    if isinstance(value, int):
        return 1
    return ""
```
```
Return type does not match returned value: expected `T`, found `Literal[1]` (invalid-return-type) [Ln 3, Col 16]
Return type does not match returned value: expected `T`, found `Literal[""]` (invalid-return-type) [Ln 4, Col 12]
```
https://play.ty.dev/2aab612b-e197-46d2-a47f-7a288cbe3574

since this is a constraint instead of a regular generic, this should be allowed

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `narrowing` added by @AlexWaygood on 2025-06-10 12:09_

---

_Label `generics` added by @AlexWaygood on 2025-06-10 12:09_

---

_Added to milestone `GA` by @carljm on 2025-07-23 23:02_

---

_Comment by @90degs2infty on 2025-09-05 14:39_

Similar to the above:

```py
def or_default[T](value: T | None, default: T) -> T:
    return default if value is None else value


def or_empty(value: str | None) -> str:
    return or_default(value, "")
```

Triggers
```
Return type does not match returned value: expected `str`, found `str | None` (invalid-return-type) [Ln 6, Col 12]
```
but to my understanding `or_default` will always return a `T`.

https://play.ty.dev/5e93f1bf-8166-4f5d-b7a2-6e4fea38265a

---

_Comment by @carljm on 2025-09-05 16:42_

The second comment here is not the same issue as the OP, it's a duplicate of #1115, which will be fixed by #623.

---
