```yaml
number: 1254
title: A class with async constructor (__new__ + __init__) reported as not awaitable
type: issue
state: open
author: dm0
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-09-25T14:24:06Z
updated_at: 2025-09-25T14:26:12Z
url: https://github.com/astral-sh/ty/issues/1254
synced_at: 2026-01-12T15:54:24Z
```

# A class with async constructor (__new__ + __init__) reported as not awaitable

---

_@dm0_

### Summary

The following sample code:
```python
import asyncio

class My:
    async def __new__(cls, *args, **kwargs):
        instance = super().__new__(cls)
        await instance.__init__(*args, **kwargs)

    async def __init__(self):
        print('__init__')

async def main():
    my = await My()

if __name__ == "__main__":
    asyncio.run(main())
```

reports `invalid-await` when checked with `ty check`:
```
r[invalid-await]: `My` is not awaitable
  --> obj.py:12:16
   |
11 | async def main():
12 |     my = await My()
   |                ^^^^
13 |
14 | if __name__ == "__main__":
   |
  ::: obj.py:3:7
   |
 1 | import asyncio
 2 |
 3 | class My:
   |       -- type defined here
 4 |     async def __new__(cls, *args, **kwargs):
 5 |         instance = super().__new__(cls)
   |
info: `__await__` is missing
info: rule `invalid-await` is enabled by default
``` 



### Version

0.0.1-alpha.21

---

_Label `type properties` added by @carljm on 2025-09-25 14:25_

---

_Label `bug` added by @carljm on 2025-09-25 14:26_

---

_Added to milestone `GA` by @carljm on 2025-09-25 14:26_

---
