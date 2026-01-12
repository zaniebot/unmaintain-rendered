```yaml
number: 743
title: "Expected `T`, found `Class`"
type: issue
state: closed
author: Kumzy
labels: []
assignees: []
created_at: 2025-07-01T16:03:38Z
updated_at: 2025-07-02T00:47:58Z
url: https://github.com/astral-sh/ty/issues/743
synced_at: 2026-01-12T15:54:23Z
```

# Expected `T`, found `Class`

---

_@Kumzy_

### Summary

Hello, I would like to know 

``litestar`` and ``polyfactory`` required there
```python
from dataclasses import dataclass
from typing import TypeVar

from polyfactory.factories.dataclass_factory import DataclassFactory

from litestar import Response, get

T = TypeVar("T")


@dataclass
class DataclassPerson:
    first_name: str
    last_name: str
    id: str

class DataclassPersonFactory(DataclassFactory[DataclassPerson]):
    __model__ = DataclassPerson


class CustomResponse(Response[T]):
    pass

@get(path="/test", name="test", signature_types=[CustomResponse])
def handler() -> CustomResponse[DataclassPerson]:
    return CustomResponse(content=DataclassPersonFactory.build())
```

```sh
uvx ty check polyfactory.py
```

Error message is:
```sh
Expected `T`, found `DataclassPerson`
```

### Version

_No response_

---

_Comment by @MatthewMckee4 on 2025-07-01 21:21_

What version are you on? I can't reproduce this on 0.0.1-alpha.12

---

_Comment by @carljm on 2025-07-02 00:47_

It reproduces for me on a12 and on main, after `uv add litestar`.

I think this is the same issue as #588. It works fine if we remove `CustomResponse` and just return a `Response` directly.

---

_Closed by @carljm on 2025-07-02 00:47_

---

_Reopened by @carljm on 2025-07-02 00:47_

---

_Closed by @carljm on 2025-07-02 00:47_

---
