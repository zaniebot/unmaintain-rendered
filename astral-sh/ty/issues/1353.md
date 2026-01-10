---
number: 1353
title: "Generic solver infers `Unknown` only when using new-style generics"
type: issue
state: closed
author: wence-
labels:
  - bug
  - generics
assignees: []
created_at: 2025-10-14T15:17:02Z
updated_at: 2026-01-08T18:41:50Z
url: https://github.com/astral-sh/ty/issues/1353
synced_at: 2026-01-10T01:51:14Z
---

# Generic solver infers `Unknown` only when using new-style generics

---

_Issue opened by @wence- on 2025-10-14 15:17_

### Summary

I think this is not reported previously.

Consider (https://play.ty.dev/79ca09f4-57e4-4544-abda-3ef3be0c08bd):

```python
from __future__ import annotations

from typing import Self, reveal_type, Protocol, TypeVar, Generic


class PayloadImplicit(Protocol):
    @classmethod
    def from_message(cls, message: MessageImplicit[Self]) -> Self:
        ...
    
class MessageImplicit[T: PayloadImplicit]:
    pass

class Implicit:
    @classmethod
    def from_message(cls, message: MessageImplicit[Self]) -> Self:
        return cls()


reveal_type(MessageImplicit[Implicit]())
reveal_type(Implicit.from_message(MessageImplicit[Implicit]()))


class PayloadExplicit(Protocol):
    @classmethod
    def from_message(cls, message: MessageExplicit[Self]) -> Self:
        ...

T = TypeVar("T", bound="PayloadExplicit")

class MessageExplicit(Generic[T]):
    pass

class Explicit:
    @classmethod
    def from_message(cls, message: MessageExplicit[Self]) -> Self:
        return cls()

reveal_type(MessageExplicit[Explicit]())
reveal_type(Explicit.from_message(MessageExplicit[Explicit]()))
```

To the best of my understanding, the definitions of `MessageImplicit` and `MessageExplicit` are structurally identical. However, `ty` (correctly) reveals that the type of `Explicit.from_message(...)` is `Explicit` whereas reveals `Implicit.from_message(...)` as `Unknown`

### Version

9090aead0

---

_Label `generics` added by @AlexWaygood on 2025-10-14 15:44_

---

_Label `Protocols` added by @carljm on 2025-10-29 21:37_

---

_Label `bug` added by @carljm on 2025-10-30 16:25_

---

_Comment by @carljm on 2025-10-30 16:29_

The [same discrepancy appears](https://play.ty.dev/51ed20b7-6546-4d4e-8b5d-e819bb6669b7) without the upper bounds on the typevars (and thus without the protocols):

```py
from __future__ import annotations

from typing import Self, reveal_type, TypeVar, Generic


class MessageImplicit[T]:
    pass

class Implicit:
    @classmethod
    def from_message(cls, message: MessageImplicit[Self]) -> Self:
        return cls()

reveal_type(MessageImplicit[Implicit]())
reveal_type(Implicit.from_message(MessageImplicit[Implicit]()))

T = TypeVar("T")

class MessageExplicit(Generic[T]):
    pass

class Explicit:
    @classmethod
    def from_message(cls, message: MessageExplicit[Self]) -> Self:
        return cls()

reveal_type(MessageExplicit[Explicit]())
reveal_type(Explicit.from_message(MessageExplicit[Explicit]()))
```

So it looks like just a problem in typevar solving. We will probably want to look at it again if it still repros after #623 

---

_Added to milestone `GA` by @carljm on 2025-10-30 16:29_

---

_Label `Protocols` removed by @carljm on 2025-10-30 16:29_

---

_Comment by @carljm on 2025-10-30 16:31_

Thanks for the report!

---

_Renamed from "Generic protocols infer `Unknown` when using new-style generics" to "Generic solver infers `Unknown` only when using new-style generics" by @carljm on 2025-10-30 16:33_

---

_Comment by @carljm on 2026-01-08 18:41_

This is fixed.

---

_Closed by @carljm on 2026-01-08 18:41_

---
