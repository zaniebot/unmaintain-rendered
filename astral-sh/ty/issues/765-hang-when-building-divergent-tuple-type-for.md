```yaml
number: 765
title: Hang when building divergent tuple type for implicit instance attributes
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - hang
assignees: []
created_at: 2025-07-04T13:12:03Z
updated_at: 2025-07-30T16:25:43Z
url: https://github.com/astral-sh/ty/issues/765
synced_at: 2026-01-12T15:54:23Z
```

# Hang when building divergent tuple type for implicit instance attributes

---

_@AlexWaygood_

### Summary

Ty hangs when checking this file:

```py
from typing import Any

class A:
    foo: tuple[Any, ...]

class B(A):
    def __init__(self, parent: "C", x: tuple[Any]):
        self.foo = parent.foo + x

class C(A):
    def __init__(self, parent: B, x: tuple[Any]):
        self.foo = parent.foo + x
```

### Version

7712c2fd1562a9d3c3d2ea8e7eb2f91b20868abe

---

_Label `bug` added by @AlexWaygood on 2025-07-04 13:12_

---

_Label `hang` added by @AlexWaygood on 2025-07-04 13:12_

---

_Comment by @sharkdp on 2025-07-08 10:07_

I think this has little to do with instance attributes, and more to do with simplification of tuple types. We correctly attempt to do fixpoint iteration to find the type of `B.foo`/`C.foo`. While doing so, we build an increasingly complex tuple type: https://play.ty.dev/5ef6c18e-e6c8-4868-bb51-92d22aea3083. Since we never simplify this to a homogenous `tuple[Any, ...]`, the iteration never converges. You can verify this by letting `ty` run for long enough on the snippet above. It should eventually panic with "too many cycle iterations" (why this takes so long is another interesting question).

Something similar happens for much simpler cases like:
```py
class A:
    def f(self: "A", flag: bool):
        if flag:
            self.foo = ()
        else:
            self.foo = self.foo + (1,)
```

The solution here, I think, will be to simplify these tuple types to their homogenous supertype when some threshold is passed.

---

_Comment by @AlexWaygood on 2025-07-08 10:13_

When it comes to addition specifically, I've actually been wondering how useful our special-casing for tuple addition really is. It causes problems for tuple subclasses, since the signature of `tuple.__add__` is inexpressible in the type system. It would be impossible to know if an `__add__` method definition on a fixed-length tuple subclass was Liskov-compatible with the `__add__` definition on its `tuple` superclass just by looking at the `__add__` signature -- so to make tuple subclasses sound, we would have to forbid ever overriding `__add__` or `__radd__` on all tuple subclasses. That feels like it might be unnecessarily restrictive if our special casing for tuple addition isn't actually that useful at the end of the day. I'd like to experiment with just ripping it out altogether.

---

_Renamed from "Hang when checking a pair of classes with mutually recursive implicit instance attributes" to "Hang when building divergent tuple type for implicit instance attributes" by @sharkdp on 2025-07-18 07:10_

---

_Closed by @AlexWaygood on 2025-07-30 16:25_

---
