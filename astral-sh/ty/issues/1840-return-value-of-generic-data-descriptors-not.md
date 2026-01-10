```yaml
number: 1840
title: Return value of generic data descriptors not infered correctly
type: issue
state: open
author: mflova
labels:
  - generics
assignees: []
created_at: 2025-12-10T12:28:24Z
updated_at: 2025-12-10T16:54:32Z
url: https://github.com/astral-sh/ty/issues/1840
synced_at: 2026-01-10T01:56:41Z
```

# Return value of generic data descriptors not infered correctly

---

_Issue opened by @mflova on 2025-12-10 12:28_

### Summary

Ther eturn value of `__get__` method is not infered properly for the following case. Mypy and pyright label them correct. I could not find this issue reported, but maybe is related to the incoming improvements for type variable inference for generic classes.

Here is a quick example:

```py
from collections.abc import Callable
from typing import Any, Generic, TypeVar

from typing_extensions import reveal_type

Value_co = TypeVar("Value_co", covariant=True)


class classproperty(Generic[Value_co]):  # noqa: N801
    def __init__(self, method: Callable[[Any], Value_co]) -> None:
        ...

    def __get__(self, instance: Any | None, owner: type[Any]) -> Value_co:
        ...


class MyClass:
    @classproperty
    def my_class_property(cls) -> str:
        return "Hello, World!"

reveal_type(MyClass.my_class_property)  # Expected: str, got: Unknown
```

### Version

ty 0.0.1-alpha.33 (35e23aa48 2025-12-09)

---

_Comment by @sharkdp on 2025-12-10 13:20_

Thank you for reporting this.

We do not support `Callable` in the generics solver yet, but will be doing so soon (https://github.com/astral-sh/ruff/pull/21551).

---

_Label `generics` added by @sharkdp on 2025-12-10 13:20_

---

_Comment by @carljm on 2025-12-10 16:47_

I checked and this example is not fixed by https://github.com/astral-sh/ruff/pull/21551, so there is something else to explore here once that lands.

---

_Added to milestone `Stable` by @carljm on 2025-12-10 16:54_

---
