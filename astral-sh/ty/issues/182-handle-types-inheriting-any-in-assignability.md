```yaml
number: 182
title: handle types inheriting Any in assignability
type: issue
state: closed
author: carljm
labels:
  - type properties
assignees: []
created_at: 2025-03-11T23:58:01Z
updated_at: 2025-07-01T06:37:53Z
url: https://github.com/astral-sh/ty/issues/182
synced_at: 2026-01-12T15:54:22Z
```

# handle types inheriting Any in assignability

---

_@carljm_

An instance of a type inheriting Any/Unknown should be assignable to any non-final type, because it might inherit from it.

Most common scenario where this occurs is `NotImplemented`; typeshed defines `_NotImplementedType` as inheriting `Any`. This is supposed to allow `NotImplemented` to be returned from any method, regardless of its annotated return type.

(Personally I am not sure this is right; only certain dunder methods really support returning `NotImplemented`. But I don't think we should spend time on anything different here right now, we should just add the support for `Any` inheritance and let the behavior fall out from typeshed.)

---

_Comment by @carljm on 2025-03-12 04:57_

Actually I think to fully support `NotImplemented` via its typeshed annotation, we'd have to support assignability of "instance-of-type-inheriting-Any" to _all_ types, which doesn't seem correct to me. Such an instance still cannot be an instance of a final type; it cannot be a literal integer or string or bool, etc. I guess other type checkers just treat types inheriting `Any` as if they were fully `Any`, but that [doesn't seem right](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcANFADJIwCmIAhgDYBQTAxg3QM6dQDCAFETgBKAFxQAdFJYAPcZRr0GAbQCMAXSgBePv2FA).

But it is important for `NotImplemented` to be returnable even from methods annotated to return a final type, or a literal type, etc. So maybe we should special-case `NotImplemented` as a `KnownInstance` instead, and handle it specially in return type checking. That doesn't seem too difficult.

---

_Renamed from "[red-knot] handle types inheriting Any in assignability" to "handle types inheriting Any in assignability" by @MichaReiser on 2025-05-07 15:26_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:27_

---

_Comment by @carljm on 2025-07-01 06:37_

It looks like [this is done](https://play.ty.dev/79b15bca-874c-40cd-8949-ea56f837b4b7).

---

_Closed by @carljm on 2025-07-01 06:37_

---
