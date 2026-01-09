---
number: 16795
title: FAST003 does not handle aliased path parameters when defined in another file
type: issue
state: open
author: HenriBlacksmith
labels:
  - type-inference
assignees: []
created_at: 2025-03-17T10:22:30Z
updated_at: 2025-03-17T13:23:41Z
url: https://github.com/astral-sh/ruff/issues/16795
synced_at: 2026-01-07T13:12:16-06:00
---

# FAST003 does not handle aliased path parameters when defined in another file

---

_Issue opened by @HenriBlacksmith on 2025-03-17 10:22_

### Summary

FAST003 rule raises errors when using aliased path parameters in an endpoint definition when the alias is declared in another Python module (another file)

# MRE

Playground: https://play.ruff.rs/ff28dc29-c8f7-4c58-9b36-b7363b8df88a (cannot add multiple files it seems, but the error is the same)

## `parameters.py`

```python 
from typing import Annotated

from pydantic import UUID4
from fastapi import Path

ExampleIdPathParameter = Annotated[
    UUID4,
    Path(
        alias="exampleId", description="The unique identifier of an example resource."
    ),
]
```

## `main.py`

```python
from fastapi import FastAPI
from pydantic import BaseModel, UUID4

from parameters import ExampleIdPathParameter

app = FastAPI()


class Example(BaseModel):
    id: UUID4
    name: str


@app.get(
    "/examples/{exampleId}",
    name="example:read",
)
async def read_example(
    example_id: ExampleIdPathParameter,
) -> Example:
    return Example(id=example_id, name=f"example-{example_id}")

```

## Command

```shell
ruff check --select FAST003 .
```

## Command output
```shell 
main.py:15:16: FAST003 Parameter `exampleId` appears in route path, but not in `read_example` signature
   |
14 | @app.get(
15 |     "/examples/{exampleId}/e",
   |                ^^^^^^^^^^^ FAST003
16 |     name="example:read",
17 | )
   |
   = help: Add `exampleId` to function signature

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

### Version

ruff 0.11.0 (2cd25ef64 2025-03-14)

---

_Comment by @MichaReiser on 2025-03-17 10:30_

Thanks. Ruff should detect aliased parameters (https://github.com/astral-sh/ruff/issues/13263) but we may miss aliased parameters when using `Annotated` with multiple elements

---

_Renamed from "FAST003 does not handle aliased path parameters" to "FAST003 does not handle aliased path parameters when defined in another file" by @HenriBlacksmith on 2025-03-17 10:37_

---

_Comment by @HenriBlacksmith on 2025-03-17 10:38_

> Thanks. Ruff should detect aliased parameters ([#13263](https://github.com/astral-sh/ruff/issues/13263)) but we may miss aliased parameters when using `Annotated` with multiple elements

Sorry, I am fixing the MRE, it actually fails when the parameter is defined in another file

---

_Label `type-inference` added by @dylwil3 on 2025-03-17 10:56_

---

_Comment by @dylwil3 on 2025-03-17 10:58_

> Sorry, I am fixing the MRE, it actually fails when the parameter is defined in another file

At the moment Ruff does not support multi-file analysis, so this extension to the rule will have to wait until then.

---

_Referenced in [astral-sh/ruff#17226](../../astral-sh/ruff/issues/17226.md) on 2025-04-07 08:25_

---
