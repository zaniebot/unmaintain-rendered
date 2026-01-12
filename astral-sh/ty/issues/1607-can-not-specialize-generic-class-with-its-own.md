```yaml
number: 1607
title: Can not specialize generic class with (its own) bound typevar in value positions
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-11-21T10:49:58Z
updated_at: 2025-11-24T17:10:55Z
url: https://github.com/astral-sh/ty/issues/1607
synced_at: 2026-01-12T15:54:25Z
```

# Can not specialize generic class with (its own) bound typevar in value positions

---

_@sharkdp_

We currently emit the following error when trying to create a generic type alias, which specializes a generic class with a legacy typevar:
```py
from typing import TypeVar, Generic

T = TypeVar("T", bound=int)

class dtype(Generic[T]): ...

# Argument to class `dtype` is incorrect: Expected `int`, found `typing.TypeVar`
MyDtype = dtype[T]
```
https://play.ty.dev/32ca2785-9c60-4134-98ec-a7817bde5bc4

This is the root cause for a large ecosystem impact in https://github.com/astral-sh/ruff/pull/21553, because numpy's `NDArray` makes use of a similar pattern.

---

_Added to milestone `Beta` by @sharkdp on 2025-11-21 10:49_

---

_Assigned to @sharkdp by @sharkdp on 2025-11-21 10:49_

---

_Label `bug` added by @sharkdp on 2025-11-21 10:50_

---

_Label `generics` added by @sharkdp on 2025-11-21 10:50_

---

_Renamed from "Can not specialize generic class with its own (bound) typevar" to "Can not specialize generic class with (its own) bound typevar" by @sharkdp on 2025-11-21 10:50_

---

_Renamed from "Can not specialize generic class with (its own) bound typevar" to "Can not specialize generic class with (its own) bound typevar in value positions" by @sharkdp on 2025-11-21 10:50_

---

_Comment by @sharkdp on 2025-11-21 15:20_

If we use `dtype[T]` as a base of another class (where it also appears in value position), this works just fine, because the class provides a typvar binding context that will turn the `T` into a bound `Type::TypeVar(…)` which *is* assignable. So the problem is really that we don't have a generic context for the implicit type alias. This is probably a strong argument in favor of giving implicit type aliases their own generic context.

---

_Comment by @carljm on 2025-11-22 01:56_

Isn't this the same as #1596 ?

---

_Closed by @sharkdp on 2025-11-24 09:03_

---

_Reopened by @carljm on 2025-11-24 17:10_

---

_Closed by @carljm on 2025-11-24 17:10_

---
