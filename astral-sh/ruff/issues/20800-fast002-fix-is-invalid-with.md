```yaml
number: 20800
title: "FAST002 fix is invalid with `...`"
type: issue
state: closed
author: CoderJoshDK
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-10T10:26:34Z
updated_at: 2025-10-21T02:47:14Z
url: https://github.com/astral-sh/ruff/issues/20800
synced_at: 2026-01-10T11:09:59Z
```

# FAST002 fix is invalid with `...`

---

_Issue opened by @CoderJoshDK on 2025-10-10 10:26_

### Summary

FAST002 converts parameters into their `Annotated` counterparts. If the source code contains an ellipses type (literal `...`) then the fix will generate incorrect semantics.

Take the following signature:

```py
@router.get("/available")
async def get_available(
    ghl_account_id: str = Query(..., description="GHL account identifier"),
) -> SystemsAvailableResponse: ...
```
The use of `...` in the default for `Query` is valid syntax (should be picked up in a lint that you shouldn't do this, but it is "valid"). 
When trying to run fix, you get:

```py
@router.get("/available")
async def get_available(
    ghl_account_id: Annotated[str, Query(description="GHL account identifier")] = ...,
) -> SystemsAvailableResponse: ...
```

When in reality, there is no default value and ruff should convert it to:

```py
@router.get("/available")
async def get_available(
    ghl_account_id: Annotated[str, Query(description="GHL account identifier")],
) -> SystemsAvailableResponse: ...
```

### Version

ruff 0.14.0

---

_Comment by @CoderJoshDK on 2025-10-10 11:00_

Minimum required repo:

```sh
uv add "fastapi[standard]"
```

```py
# main.py
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/")
async def root(name: str = Query(..., description="Your name!")):
    return {"message": f"Hello {name}"}
```

```sh
uv run fastapi dev main.py
```

If you then visit the docs page, you will find that "name" is marked as required. If you then run the fix:

```py
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/")
async def root(name: Annotated[str, Query(description="Your name!")] = ...):
    return {"message": f"Hello {name}"}
```

And visit docs now; you find name is no longer required. Implying a default. But executing the endpoint will cause an error
```
ValueError: [TypeError("'ellipsis' object is not iterable"), TypeError('vars() argument must have __dict__ attribute')]
```

So I would consider this fix as incorrect, seeing that it changes the behavior. To match behavior after fix, drop the `...`. 

---

_Comment by @ntBre on 2025-10-10 21:36_

Thanks for the report, this does seem like a bug. At first I thought this was a general type error, but as you said, the ellipsis has special meaning to fastapi, and I think we should handle that in the fix.

---

_Label `bug` added by @ntBre on 2025-10-10 21:36_

---

_Label `fixes` added by @ntBre on 2025-10-10 21:36_

---

_Closed by @ntBre on 2025-10-21 02:47_

---
