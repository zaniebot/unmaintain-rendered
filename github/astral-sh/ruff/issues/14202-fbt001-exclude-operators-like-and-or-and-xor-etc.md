---
number: 14202
title: "`FBT001` exclude operators like `__and__`, `__or__` and `__xor__`, etc."
type: issue
state: closed
author: randolf-scholz
labels:
  - rule
assignees: []
created_at: 2024-11-08T13:51:13Z
updated_at: 2024-11-10T22:40:38Z
url: https://github.com/astral-sh/ruff/issues/14202
synced_at: 2026-01-07T13:12:16-06:00
---

# `FBT001` exclude operators like `__and__`, `__or__` and `__xor__`, etc.

---

_Issue opened by @randolf-scholz on 2024-11-08 13:51_

[`FBT001`](https://docs.astral.sh/ruff/rules/boolean-type-hint-positional-argument/) should probably by default exclude methods used for operator overloading, in particular boolean logic specific operations:

- `__and__`, `__rand__`, `__iand__`
- `__or__`, `__ror__`, `__ior__`
- `__xor__`, `__rxor__`, `__ixor__`

And potentially also arithmetic operations like `__add__`, `__sub__`, `__mul__`, etc. (These may add overloads / type hints for boolean values)


---

_Referenced in [astral-sh/ruff#14203](../../astral-sh/ruff/pulls/14203.md) on 2024-11-08 14:10_

---

_Label `rule` added by @dylwil3 on 2024-11-10 00:15_

---

_Comment by @dylwil3 on 2024-11-10 00:19_

This is an interesting idea! I guess one question I have is: if you are defining one of the Boolean magic methods for `MyClass`, why would the `other` parameter be a `bool` (as opposed to `MyClass`? Similarly for arithmetic operations.

I'd be interested to see examples of this in code out in the wild.

---

_Comment by @MichaReiser on 2024-11-10 09:25_

An other option would be to ignore methods marked with @overload

---

_Comment by @randolf-scholz on 2024-11-10 10:01_

@dylwil3 A typical example are custom scalar/vector/tensor classes/protocols.

For instance, a `BooleanArray` might define `__or__(self, other: bool | Self) -> Self`.

---

_Comment by @dylwil3 on 2024-11-10 13:39_

Alright that makes sense to me- thanks for the example! I'm in favor of adopting this change.

---

_Closed by @charliermarsh on 2024-11-10 22:40_

---

_Closed by @charliermarsh on 2024-11-10 22:40_

---
