```yaml
number: 21075
title: "[FAST003] False positive on `Annotated` dependency"
type: issue
state: open
author: vinnybod
labels:
  - bug
  - rule
assignees: []
created_at: 2025-10-25T18:52:38Z
updated_at: 2025-11-10T08:29:52Z
url: https://github.com/astral-sh/ruff/issues/21075
synced_at: 2026-01-12T15:54:57Z
```

# [FAST003] False positive on `Annotated` dependency

---

_@vinnybod_

### Summary

A full reproduction can be seen at: https://github.com/vinnybod/reproductions/tree/vinnybod/FAST003


TL;DR:
If using `thing=Depends(get_thing)`, the route path parameter `thing_id` is found in the `get_thing` dependency.
If using the equivalent `thing: ThingDep`, the route path parameter is considered missing by the FAST003 rule.


```py
from typing import Annotated

from fastapi import FastAPI, Depends

app = FastAPI()


async def get_thing(thing_id: int):
    return {"thing_id": thing_id}


ThingDep = Annotated[dict, Depends(get_thing)]


# This is considered wrong by FAST003
@app.get("annotated/{thing_id}")
async def get_thing_endpoint(thing: ThingDep):
    return thing


# This is considered fine by FAST003
@app.get("depends/{thing_id}")
async def get_thing_endpoint(thing=Depends(get_thing)):
    return thing
```


```
> poetry run ruff check . --fix
FAST003 Parameter `thing_id` appears in route path, but not in `get_thing_endpoint` signature
  --> reproductions/main.py:16:21
   |
15 | # This is considered wrong by FAST003
16 | @app.get("annotated/{thing_id}")
   |                     ^^^^^^^^^^
17 | async def get_thing_endpoint(thing: ThingDep):
18 |     return thing
   |
help: Add `thing_id` to function signature

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```


### Version

ruff 0.14.1 (2bffef596 2025-10-16)

---

_Comment by @ntBre on 2025-10-27 14:27_

Thanks for the report! We may be able to try resolving these, but I think implicit type aliases like this can be a bit tricky.

---

_Label `rule` added by @ntBre on 2025-10-27 14:27_

---

_Label `needs-decision` added by @ntBre on 2025-10-27 14:27_

---

_Comment by @jaxor24 on 2025-11-10 03:10_

Also just ran into this in 0.14.3

---

_Label `needs-decision` removed by @MichaReiser on 2025-11-10 08:25_

---

_Label `bug` added by @MichaReiser on 2025-11-10 08:25_

---

_Comment by @MichaReiser on 2025-11-10 08:29_

I'd consider this a bug and the only question is how to reduce the false positives without reporting fewer true-positives. 

We could try to resolve type aliases or I think we should bail on the following line if a parameter doesn't use `Annotated` (that always means we'll have fewer names than is the case)

https://github.com/astral-sh/ruff/blob/020ff1723b428e8aa9cbcee743a6dcb9fd95cf8b/crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs#L154-L158



---
