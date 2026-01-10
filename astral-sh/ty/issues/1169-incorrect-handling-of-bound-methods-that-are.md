---
number: 1169
title: "Incorrect handling of bound methods that are overloaded on the type of `self`"
type: issue
state: open
author: sharkdp
labels:
  - bug
  - calls
  - attribute access
assignees: []
created_at: 2025-09-11T08:41:09Z
updated_at: 2026-01-09T02:49:37Z
url: https://github.com/astral-sh/ty/issues/1169
synced_at: 2026-01-10T01:48:23Z
---

# Incorrect handling of bound methods that are overloaded on the type of `self`

---

_Issue opened by @sharkdp on 2025-09-11 08:41_

### Summary

Consider the following example, where our use of `CallableSignature::bind_self` to eagerly remove the first parameter leads to a wrong display type of the bound method. The call binding goes through a different code path and leads to the correct result, though:
```py
from __future__ import annotations

from typing import reveal_type, overload


class C[T]:
    x: T

    @overload
    def method(self: C[int]) -> int:
        return 0

    @overload
    def method(self: C[str]) -> str:
        return ""

    def method(self) -> int | str:
        raise NotImplementedError


reveal_type(C[str]().method)    # ty: Overload[() -> int, () -> str]
reveal_type(C[str]().method())  # ty: str
```
https://play.ty.dev/e75a9987-fcee-4bd9-a53f-d7edf9941465

Also related to this, we do not emit an error here. It might be argued that this is fine, though, as long as we're not calling the bound method:
```py
from typing import reveal_type

class C:
    def wrong_annotation(self: int) -> int:
        return 0

reveal_type(C().wrong_annotation)
```
https://play.ty.dev/671b14e0-fa24-4ef9-a05d-8e2bfcaffe51

### Version

Current `main` (59c8fda3f)

---

_Label `bug` added by @sharkdp on 2025-09-11 08:41_

---

_Label `calls` added by @sharkdp on 2025-09-11 08:41_

---

_Label `attribute access` added by @sharkdp on 2025-09-11 08:41_

---

_Comment by @sharkdp on 2025-09-11 13:37_

(update the example above to make `C[T]` covariant in `T`, instead of bivariant, which changes the severity of this bug)

---

_Assigned to @sharkdp by @sharkdp on 2025-09-11 17:39_

---

_Unassigned @sharkdp by @sharkdp on 2025-09-30 07:58_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 02:49_

---
