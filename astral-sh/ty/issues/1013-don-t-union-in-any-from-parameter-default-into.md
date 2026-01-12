```yaml
number: 1013
title: "Don't union in `Any` from parameter default into parameter type"
type: issue
state: closed
author: shroominic
labels: []
assignees: []
created_at: 2025-08-15T17:10:24Z
updated_at: 2025-11-02T23:21:56Z
url: https://github.com/astral-sh/ty/issues/1013
synced_at: 2026-01-12T15:54:24Z
```

# Don't union in `Any` from parameter default into parameter type

---

_@shroominic_

### Summary

My pydantic model (sqlmodel) has an attribute defined as :int but ty treats it as int or Any

<img width="435" height="140" alt="Image" src="https://github.com/user-attachments/assets/44f8b3ca-96f9-491a-8a43-feeba75e3b93" />

### Version

_No response_

---

_Comment by @jelle-openai on 2025-08-15 17:12_

Could you give a code sample that shows enough code to reproduce the problem?

---

_Label `needs-mre` added by @MichaReiser on 2025-08-15 17:13_

---

_Comment by @shroominic on 2025-08-15 17:37_

```python
import os
from sqlalchemy.ext.asyncio.engine import create_async_engine
from sqlmodel import Field, SQLModel

DATABASE_URL = os.environ.get("DATABASE_URL", "sqlite+aiosqlite:///keys.db")

engine = create_async_engine(DATABASE_URL, echo=False)  # echo=True for debugging SQL


class ApiKey(SQLModel, table=True):  # type: ignore
    __tablename__ = "api_keys"

    hashed_key: str = Field(primary_key=True)
    balance: int = Field(default=0")
```


```python
from fastapi import Depends
from .db import ApiKey, AsyncSession, get_session

async def refund_wallet_endpoint(
    key: ApiKey = Depends(get_key_from_header),
    session: AsyncSession = Depends(get_session),
) -> dict:
    remaining_balance_msats = key.balance
```

---

_Comment by @carljm on 2025-08-15 18:41_

Thanks! That minification still has some undefined names, but I simplified the second file as follows:

```py
from fastapi import Depends
from db import ApiKey

async def refund_wallet_endpoint(
    key: ApiKey,
):
    remaining_balance_msats = reveal_type(key.balance)
```

And here I get the type `int` revealed.

Which makes me suspect that the `Any` component of the union is coming from `Depends(...)` resolving to `Any`.

Here's a [simpler presentation of the problem](https://play.ty.dev/6390b197-21f4-4873-87d6-1536e0e4c269).

So I don't think this is related to SQLModel fields at all; it's related to parameter type inference with defaults.

We union the default type in so that we can improve behavior in cases like `def f(x = 1): ...`, where there's no annotation but we know it can be `Literal[1]`, so we use `Unknown | Literal[1]` instead of just `Unknown`. But perhaps we should not do that when there is an annotation.

(This is similar to #136 , but for parameters instead of assignments.)

---

_Renamed from "Pydantic attributes not recognized correctly" to "Don't union in Any from parameter default into parameter type" by @carljm on 2025-08-15 18:42_

---

_Label `needs-mre` removed by @carljm on 2025-08-15 18:42_

---

_Added to milestone `Beta` by @carljm on 2025-08-15 18:43_

---

_Assigned to @carljm by @carljm on 2025-08-22 14:59_

---

_Renamed from "Don't union in Any from parameter default into parameter type" to "Don't union in `Any` from parameter default into parameter type" by @MichaReiser on 2025-09-19 08:38_

---

_Closed by @carljm on 2025-11-02 23:21_

---
