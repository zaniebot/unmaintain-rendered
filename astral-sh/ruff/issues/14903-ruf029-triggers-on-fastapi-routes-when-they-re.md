---
number: 14903
title: "RUF029 triggers on FastAPI routes when they're inner functions"
type: issue
state: closed
author: scy
labels:
  - bug
assignees: []
created_at: 2024-12-10T23:55:44Z
updated_at: 2024-12-21T14:45:29Z
url: https://github.com/astral-sh/ruff/issues/14903
synced_at: 2026-01-10T01:22:55Z
---

# RUF029 triggers on FastAPI routes when they're inner functions

---

_Issue opened by @scy on 2024-12-10 23:55_

The following code triggers [`RUF029` (unused async)](https://docs.astral.sh/ruff/rules/unused-async/):

```python
from fastapi import FastAPI


def setup_app(app: FastAPI) -> None:
    @app.get("/")
    async def get_root() -> str:  # RUF029 here
        return "Hello World!"


app = FastAPI()


@app.get("/foo")
async def get_foo() -> str:  # no error
    return "Hello this is foo"
```

Note that it doesn't trigger on the top-level function.

That's because there is special handling in the code for FastAPI, implemented by @TomerBin in #12925/#12938, but I guess the underlying cause for the different outcome is that one of the `is_fastapi_*` checks in `rules/fastapi/rules/mod.rs` doesn't detect this case correctly.

---

_Comment by @TomerBin on 2024-12-11 06:08_

Hi @scy 
Thanks for pointing that out :)
A complete solution will depend on a proper type checking in ruff, but until it'll be landed I can improve the current logic to also detect fastapi routes whose app comes from a function typed argument.

Hopefully get into that in the following days :)


---

_Label `bug` added by @MichaReiser on 2024-12-11 07:35_

---

_Referenced in [astral-sh/ruff#15093](../../astral-sh/ruff/pulls/15093.md) on 2024-12-21 12:35_

---

_Closed by @MichaReiser on 2024-12-21 14:45_

---
