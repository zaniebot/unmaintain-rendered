```yaml
number: 17226
title: "`FAST003` false positive when path parameter is included inside a `BaseModel` dependency"
type: issue
state: open
author: biellSilva
labels:
  - rule
  - type-inference
  - help wanted
assignees: []
created_at: 2025-04-05T23:16:04Z
updated_at: 2025-08-11T12:29:55Z
url: https://github.com/astral-sh/ruff/issues/17226
synced_at: 2026-01-10T11:09:58Z
```

# `FAST003` false positive when path parameter is included inside a `BaseModel` dependency

---

_Issue opened by @biellSilva on 2025-04-05 23:16_

### Summary

I’ve noticed that the rule `FAST003` (`fast-api-unused-path-parameter`) is reporting false positives when the path parameter is provided via a `BaseModel` using `Depends`.

For example, consider the following code:
```py
from fastapi import APIRouter, Depends
from pydantic import BaseModel
from typing import Annotated

router = APIRouter()

class MyParams(BaseModel):
    my_id: int

@router.get("/{my_id}")
async def get_id(params: Annotated[MyParams, Depends()]):
    return {"my_id": params.my_id}
```

In this case, `my_id` is being used correctly via the `params` dependency, but the linter still raises:
_Parameter `my_id` appears in route path, but not in `get_id` signature **Ruff [[FAST003](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter)]**_

However, this is a valid and common FastAPI pattern, where parameters are parsed from `Depends()` into a Pydantic model.

### Expected behavior
No warning should be raised when the path parameter is being handled inside a `BaseModel` via dependency injection.

### Actual behavior
The linter raises `FAST003` even though the parameter is properly used.

### Additional notes
- This pattern is especially useful for grouping parameters together.
- The rule might need to inspect `Depends()` models to detect path parameters coming from annotated dependencies.

### Versions
Ruff: 0.11.3
FastAPI: 0.115.12
Pydantic: 2.11.2

### Version

0.11.3

---

_Label `type-inference` added by @MichaReiser on 2025-04-07 08:24_

---

_Comment by @MichaReiser on 2025-04-07 08:25_

Thank's for reporting. We could improve our detection here for params that are defined in the same file but we won't be able to support this pattern when the param is defined elsewhere (see https://github.com/astral-sh/ruff/issues/16795) 

---

_Label `rule` added by @MichaReiser on 2025-04-07 08:25_

---

_Label `help wanted` added by @MichaReiser on 2025-04-07 08:25_

---

_Closed by @ntBre on 2025-06-02 18:34_

---

_Comment by @ntBre on 2025-06-02 18:35_

Oops, reopening this for more robust support with type inference, but the same-file case should now be resolved!

---

_Reopened by @ntBre on 2025-06-02 18:35_

---

_Comment by @phi-friday on 2025-08-11 12:29_

It seems that this case is also a false positive.

```python
from __future__ import annotations

from typing import Annotated, Any

from fastapi import APIRouter, Depends, Path

router = APIRouter()


async def find_item(id: Annotated[str, Path()]) -> str:  # noqa: A002
    """Find an item by its ID."""
    return f"Item with ID: {id}"


FindItem = Depends(find_item)


@router.get("/{id}")
async def get_item(item: Annotated[str, FindItem]) -> dict[str, Any]:
    """Get an item by its ID."""
    return {"item": item}

```

```zsh
❯ uv run ruff check test.py
test.py:18:15: FAST003 Parameter `id` appears in route path, but not in `get_item` signature
   |
18 | @router.get("/{id}")
   |               ^^^^ FAST003
19 | async def get_item(item: Annotated[str, FindItem]) -> dict[str, Any]:
20 |     """Get an item by its ID."""
   |
   = help: Add `id` to function signature

Found 1 error.
```

---
