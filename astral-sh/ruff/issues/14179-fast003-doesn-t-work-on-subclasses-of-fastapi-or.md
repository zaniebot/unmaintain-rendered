```yaml
number: 14179
title: "FAST003 doesn't work on subclasses of FastAPI or APIRouter"
type: issue
state: closed
author: fraser-langton
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-11-07T23:11:58Z
updated_at: 2024-11-13T21:51:08Z
url: https://github.com/astral-sh/ruff/issues/14179
synced_at: 2026-01-12T15:54:53Z
```

# FAST003 doesn't work on subclasses of FastAPI or APIRouter

---

_@fraser-langton_

We use custom classes for our app and router objects, wondering if it was possible for that to be picked up?

```python
from fastapi import APIRouter, FastAPI


class CustomAPI(FastAPI): ...


class CustomRouter(APIRouter): ...


app = CustomAPI()
router = CustomRouter()


@app.get("/things_1/{thing_id}")
async def read_thing_1(query: str): ...


@router.get("/things_2/{thing_id}")
async def read_thing_2(query: str): ...
```

currently `ruff check .` yields no errors - if I change to using the base classes it throws an error


---

_Label `rule` added by @MichaReiser on 2024-11-08 08:19_

---

_Label `type-inference` added by @MichaReiser on 2024-11-08 08:19_

---

_Comment by @MichaReiser on 2024-11-08 08:22_

Hi @fraser-langton 

Ruff's current capabilities to reason about class inheritance is very limited and ad hoc. It doesn't work at all if the types are defined in different modules. We're working on a new analysis backend that will support this long-term but it's still far out. 

Short term, we could consider introducing a setting (or multiple) similar to [`logger-objects`](https://docs.astral.sh/ruff/settings/#lint_logger-objects) that lists all custom fast api base classes. Note, the setting should be plugin-specific and not in the global `lint` section.

---

_Comment by @fraser-langton on 2024-11-08 08:33_

@MichaReiser makes complete sense, thanks for the advice 

---

_Closed by @fraser-langton on 2024-11-13 21:51_

---
