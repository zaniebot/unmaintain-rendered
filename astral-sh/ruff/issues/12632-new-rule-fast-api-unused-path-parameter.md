---
number: 12632
title: "New rule: fast-api-unused-path-parameter"
type: issue
state: closed
author: Matthieu-LAURENT39
labels:
  - rule
assignees: []
created_at: 2024-08-02T14:35:31Z
updated_at: 2025-01-07T14:52:04Z
url: https://github.com/astral-sh/ruff/issues/12632
synced_at: 2026-01-10T01:22:52Z
---

# New rule: fast-api-unused-path-parameter

---

_Issue opened by @Matthieu-LAURENT39 on 2024-08-02 14:35_

Keywords: fastapi, path parameters

Hello, i've been using ruff for a while and i'm really loving it.
I saw that recently some FastAPI rules have been added, and i would like to suggest a new one, `fast-api-unused-path-parameter`.
It's pretty simple, it verifies that when defining a route that takes in path parameters, the function has matching arguments.

For example, the following code would trigger the rule, as we define a path parameter named "thing_id", but the function doesn't have an argument named "thing_id":
```py
from fastapi import FastAPI

app = FastAPI()

@app.get("/things/{thing_id}")
async def read_thing(id: int, query: str = None):
    return {"thing_id": id, "query": query}
```

Also of note is that it should ignore any characters after a `:` in the path parameter name, so this wouldn't trigger the rule:
```py
...

@app.get("/place/{my_path:path}")
def read_path(my_path):
    ...
```

If this rule is a good fit for ruff, i'd love to make a PR to implement it!

---

_Renamed from "New rule: fast-api-unused-path-parameters" to "New rule: fast-api-unused-path-parameter" by @Matthieu-LAURENT39 on 2024-08-02 14:35_

---

_Referenced in [astral-sh/ruff#12638](../../astral-sh/ruff/pulls/12638.md) on 2024-08-02 18:10_

---

_Label `rule` added by @charliermarsh on 2024-08-03 12:23_

---

_Comment by @mattmess1221 on 2024-08-17 00:10_

How should routes with path parameters consumed in dependencies behave? Example:
```py
def foo(bar: Annotated[str, Path()]):
  print(bar)


@app.get("/foo/{bar}", dependencies=[Depends(foo)])
async def do_foo():
    pass
```

---

_Comment by @MichaReiser on 2024-08-17 06:30_

Thanks for proposing a new rule.

Can you help me understand what's still missing from https://github.com/astral-sh/ruff/pull/12638 that shipped as preview rule in 0.6.1?

---

_Comment by @AlexWaygood on 2025-01-07 14:52_

Implemented in https://github.com/astral-sh/ruff/pull/12638

---

_Closed by @AlexWaygood on 2025-01-07 14:52_

---
