```yaml
number: 1700
title: "Infered generic type doesn't respect boundary"
type: issue
state: closed
author: kalekseev
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-01T11:03:03Z
updated_at: 2026-01-08T04:02:54Z
url: https://github.com/astral-sh/ty/issues/1700
synced_at: 2026-01-10T01:56:40Z
```

# Infered generic type doesn't respect boundary

---

_Issue opened by @kalekseev on 2025-12-01 11:03_

### Summary

This pattern used in django stubs 

```py
import uuid
from typing import TYPE_CHECKING, Generic, Literal, TypeVar, overload, reveal_type

if TYPE_CHECKING:
    from collections.abc import Callable

_U = TypeVar("_U", bound=uuid.UUID | None)

class UUIDField(Generic[_U]):
    @overload
    def __new__(
        cls,
        *,
        null: Literal[False] = False,
        default: _U | Callable[[], _U] | None = ...,
    ) -> UUIDField[uuid.UUID]: ...
    @overload
    def __new__(
        cls,
        *,
        null: Literal[True],
        default: _U | Callable[[], _U] = ...,
    ) -> UUIDField[uuid.UUID | None]: ...

class Model:
    field = UUIDField(default=uuid.uuid4)
    field_null = UUIDField(null=True)

reveal_type(Model().field)
reveal_type(Model().field_null)
```

pyright output: 

```
information: Type of "Model().field" is "UUIDField[UUID]"
information: Type of "Model().field_null" is "UUIDField[UUID | None]"
```

ty output

```
info[revealed-type]: Revealed type
  --> a.pyi:31:13
   |
29 |     field_null = UUIDField(null=True)
30 |
31 | reveal_type(Model().field)
   |             ^^^^^^^^^^^^^ `UUIDField[() -> UUID]`
32 | reveal_type(Model().field_null)
   |

info[revealed-type]: Revealed type
  --> a.pyi:32:13
   |
31 | reveal_type(Model().field)
32 | reveal_type(Model().field_null)
   |             ^^^^^^^^^^^^^^^^^^ `UUIDField[Unknown]
```



### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Label `bug` added by @sharkdp on 2025-12-01 11:14_

---

_Label `generics` added by @sharkdp on 2025-12-01 11:14_

---

_Comment by @sharkdp on 2025-12-01 11:15_

Thank you for reporting this.

This might be solved by https://github.com/astral-sh/ruff/pull/21551?

---

_Comment by @carljm on 2025-12-02 02:51_

I think the more fundamental issue here is #281 . Pyright is (correctly) preferring the return type of `__new__` (which in this case is concrete, does not include a type variable) over anything inferred from the call. Mypy does the same.



---

_Closed by @carljm on 2025-12-02 02:51_

---

_Comment by @kalekseev on 2026-01-08 04:02_

JFYI this problem is fixed in 0.0.10, the only django types specific problem I see in my codebase is None assignement to nullable field ie `model.filed = None` leads to ```Invalid assignment to data descriptor attribute `field` on type `Self@method` with custom `__set__` method```

---
