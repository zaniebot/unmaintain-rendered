---
number: 4171
title: Consider making RUF009 work with custom dataclasses
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-05-01T20:04:17Z
updated_at: 2023-07-10T01:29:48Z
url: https://github.com/astral-sh/ruff/issues/4171
synced_at: 2026-01-07T13:12:14-06:00
---

# Consider making RUF009 work with custom dataclasses

---

_Issue opened by @NeilGirdhar on 2023-05-01 20:04_

Ruff produces useful errors for dataclasses.  Recently, [PEP 681](https://peps.python.org/pep-0681/) added the ability to define custom dataclasses.  However, Ruff doesn't treat these as dataclasses and produces no errors:
```python
from dataclasses import field
import dataclasses as dc
from collections.abc import Callable
from typing import Any, TypeVar

from typing_extensions import dataclass_transform

_T = TypeVar('_T')

def model_field(*,
                default: None | Any = ...,
                resolver: None | Callable[[], Any] = None,
                init: bool = True
                ) -> Any:
    return field(default=default, init=init)

@dataclass_transform(
    kw_only_default=True,
    field_specifiers=(model_field,))
def create_model(*,
                 init: bool = True,
                 ) -> Callable[[type[_T]], type[_T]]:
    return dc.dataclass(init=init)


def f(x: int) -> int:
    return x + 1


@create_model(init=False)
class CustomerModel:
    a: int = model_field(resolver=lambda: 0)  # Should be fine: model_field is a field specifier
    b: int = f(1)  # Should give RUF009


c = CustomerModel()
```

---

_Label `rule` added by @charliermarsh on 2023-05-02 00:54_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:29_

---

_Referenced in [astral-sh/ruff#15345](../../astral-sh/ruff/pulls/15345.md) on 2025-01-08 10:34_

---
