```yaml
number: 1166
title: Inconsistent import handling of aiohttp.web
type: issue
state: closed
author: hynek
labels: []
assignees: []
created_at: 2025-09-10T16:43:08Z
updated_at: 2025-09-10T17:06:53Z
url: https://github.com/astral-sh/ty/issues/1166
synced_at: 2026-01-12T15:54:24Z
```

# Inconsistent import handling of aiohttp.web

---

_@hynek_

### Summary

So still https://github.com/hynek/svcs/pull/141, I've run into weirdness where Ty end up fighting Ruff. :)

```python
from aiohttp import web

reveal_type(web)
```

reveals `object`

while

```python
import aiohttp.web as web

reveal_type(web)
```

reveals `<module 'aiohttp.web'>` (and Ruff complains with PLR0402) as does


```python
import aiohttp

reveal_type(aiohttp.web)
```


Here's the module: https://github.com/aio-libs/aiohttp/blob/master/aiohttp/web.py


### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @carljm on 2025-09-10 16:53_

Thanks for the report! This is #1053; see https://github.com/astral-sh/ty/issues/1053#issuecomment-3201578027 for a description of what's happening. (In this case aiohttp has a module `__getattr__` annotated to return `object`.)

---

_Closed by @carljm on 2025-09-10 16:53_

---

_Comment by @hynek on 2025-09-10 16:58_

bleh sorry and I've searched for aiohttp because I didn't know what else to look for :(

---

_Comment by @carljm on 2025-09-10 17:06_

No worries at all, we appreciate any and all reports! Many duplicates are quite difficult to find.

---
