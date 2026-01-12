```yaml
number: 462
title: Create separate representations for specialized and unspecialized function literals
type: issue
state: open
author: dcreager
labels:
  - type properties
assignees: []
created_at: 2025-05-20T16:24:48Z
updated_at: 2025-11-14T14:42:09Z
url: https://github.com/astral-sh/ty/issues/462
synced_at: 2026-01-12T15:54:23Z
```

# Create separate representations for specialized and unspecialized function literals

---

_@dcreager_

We currently represent "class literals" and "class types" differently, which differ in how they handle generic classes. For a generic class, the class literal is unspecialized, and the class type is specialized.

We did not do the same thing for functions, thinking that it wasn't needed since specialized functions are ephemeral, only needed for the duration of checking a particular call of the specialized function. This is not true, though, for a method of a generic class, which should have the specialization of the class applied to its signature. (It's possible to "persist" this "generic" function via e.g. `C[int].method`.)

We should update the function representation to encode the same distinction. This will help with the property test failure in https://github.com/astral-sh/ty/issues/459.

---

_Label `type properties` added by @AlexWaygood on 2025-05-20 17:59_

---

_Comment by @dcreager on 2025-05-21 14:14_

Copying some notes from an in-person discussion we just had at the PyCon sprint.

It will be important to recognize that the inhabitants of a type are not "runtime objects", they're "runtime objects with some disambiguation (mumble mumble)". As a concrete example, given

```py
class C[T]:
    @staticmethod
    def method(x: T) -> None: ...
```

`C[int].method` and `C[str].method` are runtime identical (same physical object; equivalent per `id()` and `is`). But they do not have the same behavior according to the type system, since `C[int].method("string")` is an invalid call but `C[str].method("string")` is valid. Since they have different behavior, they cannot be (type system) equivalent, nor subtypes of each other (in either direction), even though they are runtime equivalent. So runtime equivalence cannot be the same thing as type-system equivalence.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:42_

---
