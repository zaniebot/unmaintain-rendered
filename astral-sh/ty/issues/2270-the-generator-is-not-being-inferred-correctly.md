```yaml
number: 2270
title: The generator is not being inferred correctly.
type: issue
state: closed
author: phi-friday
labels: []
assignees: []
created_at: 2025-12-30T05:22:24Z
updated_at: 2025-12-30T07:54:04Z
url: https://github.com/astral-sh/ty/issues/2270
synced_at: 2026-01-12T15:54:26Z
```

# The generator is not being inferred correctly.

---

_@phi-friday_

### Summary

```python
from collections.abc import AsyncGenerator, Generator
import asyncio

def gen() -> Generator[int]:
    yield "test"
    
async def agen() -> AsyncGenerator[int]:
    await asyncio.sleep(0)
    yield "test"
```
https://play.ty.dev/66420063-05a9-452f-a61d-da52a4a90daf

Is this related to issue #1718, or is it a different issue?

### Version

playground b925ae506

---

_Comment by @MichaReiser on 2025-12-30 07:54_

Yes, this is https://github.com/astral-sh/ty/issues/1718

ty doesn't support yield at the moment. 

---

_Closed by @MichaReiser on 2025-12-30 07:54_

---
