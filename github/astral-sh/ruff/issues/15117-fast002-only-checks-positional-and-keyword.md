---
number: 15117
title: "`FAST002` only checks positional and keyword parameters"
type: issue
state: closed
author: harupy
labels:
  - rule
assignees: []
created_at: 2024-12-23T03:39:12Z
updated_at: 2024-12-23T10:02:43Z
url: https://github.com/astral-sh/ruff/issues/15117
synced_at: 2026-01-07T13:12:16-06:00
---

# `FAST002` only checks positional and keyword parameters

---

_Issue opened by @harupy on 2024-12-23 03:39_

`FAST002` only checks positional and keyword parameters. Should it also check keyword-only parameters?

```python
from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(*, item_id: int = Path(title="The ID of the item to get"), q: str):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

Source: https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/#order-the-parameters-as-you-need-tricks

---

_Comment by @dhruvmanila on 2024-12-23 04:14_

Yeah, that makes sense.

---

_Label `rule` added by @dhruvmanila on 2024-12-23 04:14_

---

_Referenced in [astral-sh/ruff#15119](../../astral-sh/ruff/pulls/15119.md) on 2024-12-23 08:07_

---

_Closed by @dhruvmanila on 2024-12-23 10:02_

---
