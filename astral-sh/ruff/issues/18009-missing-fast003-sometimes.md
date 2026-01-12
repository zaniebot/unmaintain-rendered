```yaml
number: 18009
title: Missing FAST003 sometimes
type: issue
state: open
author: cdeil
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-05-10T20:37:54Z
updated_at: 2025-06-11T07:18:31Z
url: https://github.com/astral-sh/ruff/issues/18009
synced_at: 2026-01-12T15:54:56Z
```

# Missing FAST003 sometimes

---

_@cdeil_

### Summary

I noticed that ruff doesn't emit FAST003 in our codebase in places where it should.

Example:
```python
from fastapi import APIRouter, Depends
from elsewhere import get_user

router = APIRouter()

@router.get("/{address_id}/{user_id}")
def get_address(
    address_id: int,
    current_user: str = Depends(get_user),
):
    ...
```

This should raise the following error:
```
FAST003 Parameter `user_id` appears in route path, but not in `get_address` signature
```

But it currently doesn't:
```
% ruff check --select FAST003 example.py 
All checks passed!
```

The problem is related to the `get_user` function dependency. If I define it in the same file then FAST003 is correctly emitted.

This is weird because the error relating to missing `user_id` parameter in the signature should not depend on the unrelated parameter `current_user` or its dependency.

Could you please make it better and emit FAST003 in this case?

### Version

0.11.9

---

_Comment by @dylwil3 on 2025-05-11 20:36_

If I understand correctly, the reason the lint is elided here is to avoid something like this (modified from #13657):

```python
from fastapi import FastAPI, Depends
from typing import Annotated
from elsewhere import foo

app = FastAPI()

@app.get("/things/{thing_id}")
async def read_thing(other: Annotated[str, Depends(foo)]):
    return other
```

where `foo` is defined in a separate file as:

```python
def foo(thing_id: str):
    return thing_id
```

Since Ruff does not perform multi-file analysis, there is no way to know whether or not `foo` accepted `thing_id` as a parameter.

---

_Label `question` added by @dylwil3 on 2025-05-11 20:37_

---

_Label `type-inference` added by @MichaReiser on 2025-05-12 07:01_

---

_Label `question` removed by @MichaReiser on 2025-06-11 07:18_

---

_Label `rule` added by @MichaReiser on 2025-06-11 07:18_

---
