```yaml
number: 874
title: Equivalence of single-member enums not fully implemented when deeply nested inside other types
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - type properties
  - enums
assignees: []
created_at: 2025-07-22T18:15:03Z
updated_at: 2025-09-23T20:01:52Z
url: https://github.com/astral-sh/ty/issues/874
synced_at: 2026-01-12T15:54:24Z
```

# Equivalence of single-member enums not fully implemented when deeply nested inside other types

---

_@AlexWaygood_

### Summary

```py
from enum import Enum
from ty_extensions import static_assert, is_equivalent_to
from typing import Literal, reveal_type, Protocol

class Foo(Enum):
    X = 1

static_assert(
    is_equivalent_to(
        tuple[Foo] | int | str,
        str | int | tuple[Literal[Foo.X]]
    )
)

class Bar(Protocol):
    a: Foo

class Baz(Protocol):
    a: Literal[Foo.X]

static_assert(is_equivalent_to(Bar, Baz))
```

Both these `static_assert` calls fail currently, but they should both succeed. I think this should be easily fixable by fiddling with the implementation of `Type::normalized()` for enums: either we normalize `Literal[Foo.X]` to `Foo` if the enum only has a single member, or we normalize `Foo` to `Literal[Foo.X]` if `Foo` only has a single member.

https://play.ty.dev/441ade21-3807-4571-a0de-e8f317a08599

---

_Label `bug` added by @AlexWaygood on 2025-07-22 18:15_

---

_Label `type properties` added by @AlexWaygood on 2025-07-22 18:15_

---

_Renamed from "Equivalence of single-member enums not fully implemented when nested inside other types" to "Equivalence of single-member enums not fully implemented when deeply nested inside other types" by @AlexWaygood on 2025-07-22 18:41_

---

_Assigned to @sharkdp by @sharkdp on 2025-07-23 07:17_

---

_Comment by @sharkdp on 2025-07-23 07:18_

Thanks, Alex!

> I think this should be easily fixable by fiddling with the implementation of `Type::normalized()`

Indeed: https://github.com/astral-sh/ruff/pull/19502

---

_Closed by @sharkdp on 2025-07-23 08:14_

---

_Label `enums` added by @AlexWaygood on 2025-09-23 20:01_

---
