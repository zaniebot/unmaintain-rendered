```yaml
number: 16651
title: "[red-knot] support dataclasses"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-03-11T23:50:07Z
updated_at: 2025-05-07T15:18:16Z
url: https://github.com/astral-sh/ruff/issues/16651
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] support dataclasses

---

_Issue opened by @carljm on 2025-03-11 23:50_

_No description provided._

---

_Label `red-knot` added by @carljm on 2025-03-11 23:50_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Comment by @sharkdp on 2025-04-02 13:26_

What do we plan here *in a first iteration*? We have no support at all for data classes, but we also don't raise a lot of false positives at the moment(?). Attribute annotations work just like for normal classes:
```py
from dataclasses import dataclass

@dataclass
class C:
    x: int
    y: str | None = None

c = C(1, "foo")  # This call is completely unchecked; so no false positives, but also no true positives

reveal_type(c.x)  # revealed: int
reveal_type(c.y)  # revealed: str | None
```

We currently *do* report false positives if there are generated special methods whose existence we do not model:
```py
@dataclass(order=True)
class C:
    x: int

C(2) > C(1)  # False positive: Operator `>` is not supported for types `C` and `C`
```

---

_Comment by @carljm on 2025-04-02 15:05_

I think the highest priority item would be correctly synthesizing the `__init__` method, because if/when constructor-call-checking lands, it will cause lots of false positives on dataclasses if we don't have that.

---

_Comment by @AlexWaygood on 2025-04-07 10:14_

Agreed that synthesising the `__init__` method should definitely be the highest-priority item.

> We currently _do_ report false positives if there are generated special methods whose existence we do not model:
> 
> @dataclass(order=True)
> class C:
>     x: int
> 
> C(2) > C(1)  # False positive: Operator `>` is not supported for types `C` and `C`

I think the second-highest priority item might be synthesising the `__lt__`, `__le__`, `__gt__` and `__ge__` methods for `order=True`, to address this false positive here.

---

_Assigned to @sharkdp by @carljm on 2025-04-24 15:36_

---

_Comment by @sharkdp on 2025-04-28 08:14_

Initial support for dataclasses has been implemented. See https://github.com/astral-sh/ty/issues/111 for a lower-priority follow-up ticket.

---

_Comment by @AlexWaygood on 2025-04-28 10:30_

I'll close this for now so that it no longer shows up on the alpha milestone of tasks still to be done in the next fortnight. Thanks @sharkdp! 😃

---

_Closed by @AlexWaygood on 2025-04-28 10:30_

---
