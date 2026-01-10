```yaml
number: 1715
title: Error for literal addition in loop
type: issue
state: closed
author: vlashada
labels: []
assignees: []
created_at: 2025-12-02T01:22:19Z
updated_at: 2025-12-02T01:25:30Z
url: https://github.com/astral-sh/ty/issues/1715
synced_at: 2026-01-10T01:58:59Z
```

# Error for literal addition in loop

---

_Issue opened by @vlashada on 2025-12-02 01:22_

### Summary

Adding to Literal[1] inside a for loop reveals type to be Literal[2] and not int

https://play.ty.dev/3f8d1535-a773-4d06-a589-ad3115bf79ef

```python
from typing import reveal_type
res = 1
for i in range(10):
    res = res + 1
    reveal_type(res)
```

### Version

ec854c719

---

_Comment by @carljm on 2025-12-02 01:25_

Thanks for the report! Yes, this is #232 , which we plan to fix before stable release.

---

_Closed by @carljm on 2025-12-02 01:25_

---
