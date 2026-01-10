```yaml
number: 13657
title: "FAST003 doesn't detect params used in injected dependencies"
type: issue
state: closed
author: cbornet
labels:
  - bug
  - rule
  - help wanted
  - preview
assignees: []
created_at: 2024-10-07T10:27:10Z
updated_at: 2025-01-13T08:51:04Z
url: https://github.com/astral-sh/ruff/issues/13657
synced_at: 2026-01-10T11:09:55Z
```

# FAST003 doesn't detect params used in injected dependencies

---

_Issue opened by @cbornet on 2024-10-07 10:27_

It seems that it would be possible for FAST003 to detect params used in injected dependencies.

* List of keywords you searched for before creating this issue: `FAST003`
* A minimal code snippet that reproduces the bug.
```python
from fastapi import FastAPI, Depends
from typing import Annotated

app = FastAPI()

def foo(thing_id: str):
    return thing_id

@app.get("/things/{thing_id}")
async def read_thing(other: Annotated[str, Depends(foo)]):
    return other
```

* The command you invoked 
```sh
ruff check --select "FAST" --preview bar.py --isolated
bar.py:9:19: FAST003 Parameter `thing_id` appears in route path, but not in `read_thing` signature
   |
 7 |     return thing_id
 8 |
 9 | @app.get("/things/{thing_id}")
   |                   ^^^^^^^^^^ FAST003
10 | async def read_thing(other: Annotated[str, Depends(foo)]):
11 |     return other
   |
   = help: Add `thing_id` to function signature
```
* The current Ruff version (`ruff --version`)
```sh
ruff --version
ruff 0.6.9
```

I would expect FAST003 to not be triggered in the snippet since `thing_id` is used in injected `foo` callable.




---

_Label `bug` added by @MichaReiser on 2024-10-07 12:35_

---

_Label `rule` added by @MichaReiser on 2024-10-07 12:35_

---

_Label `preview` added by @MichaReiser on 2024-10-07 12:35_

---

_Comment by @MichaReiser on 2025-01-08 08:21_

That makes sense, thanks. An easy fix could be to skip the rule when we see a `Depends` annotation. A somewhat more elaborated fix would be to resolve the `Depends`'s target and try to see if it has any parameter with the given name.

---

_Label `help wanted` added by @MichaReiser on 2025-01-08 08:21_

---

_Closed by @MichaReiser on 2025-01-13 08:51_

---
