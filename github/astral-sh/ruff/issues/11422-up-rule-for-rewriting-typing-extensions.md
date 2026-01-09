---
number: 11422
title: "UP: Rule for rewriting `typing_extensions.TypeAliasType` as `type ...` on Python 3.12"
type: issue
state: closed
author: tmke8
labels:
  - rule
assignees: []
created_at: 2024-05-14T10:56:55Z
updated_at: 2024-05-26T19:05:36Z
url: https://github.com/astral-sh/ruff/issues/11422
synced_at: 2026-01-07T13:12:15-06:00
---

# UP: Rule for rewriting `typing_extensions.TypeAliasType` as `type ...` on Python 3.12

---

_Issue opened by @tmke8 on 2024-05-14 10:56_

On Python 3.12, the new type alias syntax, `type X = Y`, produces a `typing.TypeAliasType` object, which has been backported to `typing_extensions`.

And pydantic makes use of this `typing_extensions.TypeAliasType`: https://docs.pydantic.dev/latest/concepts/types/#named-type-aliases

From the pydantic docs:
```python
from typing import List, TypeVar

from annotated_types import Gt
from typing_extensions import Annotated, TypeAliasType

T = TypeVar('T')  # or a `bound=SupportGt`

PositiveList = TypeAliasType(
    'PositiveList', List[Annotated[T, Gt(0)]], type_params=(T,)
)
```

(Why aren't they just using `TypeAlias`? Because then pydantic can't access the name of the alias. And there are also problems with recursive aliases.)

It would be nice if this could automatically be rewritten to
```python
from annotated_types import Gt
from typing import Annotated

type PositiveList[T] = list[Annotated[T, Gt(0)]]
```
on Python 3.12.

This should be an entirely _safe_ fix, because it's just different syntax to produce the same result.

---

_Label `rule` added by @zanieb on 2024-05-14 11:43_

---

_Comment by @zanieb on 2024-05-14 11:43_

Makes sense to me!

---

_Referenced in [astral-sh/ruff#11530](../../astral-sh/ruff/pulls/11530.md) on 2024-05-24 10:47_

---

_Closed by @charliermarsh on 2024-05-26 19:05_

---
