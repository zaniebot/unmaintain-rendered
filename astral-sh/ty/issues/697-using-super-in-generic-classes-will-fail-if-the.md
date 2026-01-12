```yaml
number: 697
title: "Using super() in generic classes will fail if the `self` parameter type is set"
type: issue
state: closed
author: Glyphack
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-24T22:23:04Z
updated_at: 2025-10-13T10:57:48Z
url: https://github.com/astral-sh/ty/issues/697
synced_at: 2026-01-12T15:54:23Z
```

# Using super() in generic classes will fail if the `self` parameter type is set

---

_@Glyphack_

### Summary

The following code emits a diagnostic while it looks fine:

```py
class A[T]:
    def f(self: "A", a: T) -> T:
        return a

class B[T](A[T]):
    def f(self: "B", a: T) -> T:
        # `B[Unknown]` is not an instance or subclass of `<class 'B'>` in `super(<class 'B'>, B[Unknown])` call (invalid-super-argument) [Ln 9, Col 16]
        return super().f(a)
```
[playground](https://play.ty.dev/04f9aa93-a7e8-4ea4-a897-9fc59d5790e6), while [pyright](https://pyright-play.net/?code=MYGwhgzhAECCDaAVAugLgFDS9AJgUwDNoCAKCPEA1aAIlhoBpoxrEBKaAWgD5pENsg6ACc8AFwCuwgHbN06UJBgAhJMhIIUbAdnxFS5StRrLGzVhx58dQkeKmyIEgA55hJNgDpSYNkA) accepts this code.

I also tried annotating self with other types:

With specialized instance: [playground](https://play.ty.dev/dd37758e-0a87-4143-b571-e536396933f2)
With Self type: [playground](https://play.ty.dev/b5bcb61e-dce1-4d22-8496-1d87ef1a0f67)
(Currently the self type example will not pass even with a non-generic too. But that will be fixed once we have implicit self.)

The problem seems to be in the `Specilization::has_relation_to` method because it returns false.

### Version

9d8cba4e8 2025-06-25

---

_Renamed from "Using super() in generic classes will fail if the `self` parameter type is set." to "Using super() in generic classes will fail if the `self` parameter type is set" by @Glyphack on 2025-06-24 22:27_

---

_Label `generics` added by @carljm on 2025-06-25 05:49_

---

_Label `bug` added by @AlexWaygood on 2025-10-06 16:14_

---

_Closed by @AlexWaygood on 2025-10-13 10:57_

---
