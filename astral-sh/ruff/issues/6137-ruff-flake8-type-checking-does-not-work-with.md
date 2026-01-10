---
number: 6137
title: "`ruff.flake8-type-checking` Does not work with `FastAPI`"
type: issue
state: closed
author: Tomperez98
labels: []
assignees: []
created_at: 2023-07-27T23:42:34Z
updated_at: 2023-08-04T02:29:43Z
url: https://github.com/astral-sh/ruff/issues/6137
synced_at: 2026-01-10T01:22:45Z
---

# `ruff.flake8-type-checking` Does not work with `FastAPI`

---

_Issue opened by @Tomperez98 on 2023-07-27 23:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Context

Command: `ruff --fix-only src`
```toml
[tool.ruff.flake8-type-checking]
runtime-evaluated-base-classes = [
    "pydantic.BaseModel",
    "mashumaro.mixins.orjson.DataClassORJSONMixin",
    "fastapi.responses.Response",
]
runtime-evaluated-decorators = ["fastapi.APIRouter.get", "router.get"]
```
Ruff version: `ruff 0.0.278`

```python3
if TYPE_CHECKING:
    import uuid

@router.get(path="/{user_id}")
async def get_user(user_id: uuid.UUID) -> Response:
    ...
```

## Error message

```bash
pydantic.errors.PydanticUndefinedAnnotation: name 'uuid' is not defined
```

## Expected behavior

```python3
import uuid
```
should be moved out from the `TYPE_CHECKING` block since is used by fastapi at runtime. I don't know how to configure ruff to fix thin

---

_Comment by @charliermarsh on 2023-07-28 00:27_

Is `router` imported from another file?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-28 01:26_

---

_Comment by @Tomperez98 on 2023-07-28 16:12_

```python3
from fastapi import APIRouter

if TYPE_CHECKING:
    import uuid

router = APIRouter()

@router.get(path="/{user_id}")
async def get_user(user_id: uuid.UUID) -> Response:
    ...

---

_Comment by @Tomperez98 on 2023-07-28 16:12_

@charliermarsh Sorry, I wasn't clear. But it's defined within the same file

---

_Label `waiting-on-author` removed by @zanieb on 2023-07-28 20:23_

---

_Comment by @charliermarsh on 2023-08-04 02:29_

The best we can support right now is: put your router in a separate file (e.g., assume it's called `routers.py`), then import it where you use it, like:

```python
from routers import router

if TYPE_CHECKING:
    import uuid

@router.get(path="/{user_id}")
async def get_user(user_id: uuid.UUID) -> Response:
    ...
```

Then, you can mark it as runtime-required with:

```toml
[tool.ruff.flake8-type-checking]
runtime-evaluated-decorators = ["routers.router.get"]
```

In short, the paths included the configuration file need to be paths to imported symbols, rather than just plain names. It's a known limitation (https://github.com/astral-sh/ruff/issues/5486) right now that the symbol has to be defined in another file and imported in order to match, but it goes beyond this issue I think. Hopefully this is somewhat helpful though.


---

_Closed by @charliermarsh on 2023-08-04 02:29_

---

_Referenced in [astral-sh/ruff#9312](../../astral-sh/ruff/issues/9312.md) on 2023-12-29 18:43_

---
