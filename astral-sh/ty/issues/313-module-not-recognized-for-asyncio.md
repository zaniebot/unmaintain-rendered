```yaml
number: 313
title: "Module not recognized for `asyncio`"
type: issue
state: closed
author: ion-elgreco
labels: []
assignees: []
created_at: 2025-05-10T17:15:45Z
updated_at: 2025-05-10T17:26:05Z
url: https://github.com/astral-sh/ty/issues/313
synced_at: 2026-01-10T02:34:09Z
```

# Module not recognized for `asyncio`

---

_Issue opened by @ion-elgreco on 2025-05-10 17:15_

### Summary

```python
import asyncio

print(asyncio.subprocess.PIPE)
```
returns
```python
error[unresolved-attribute]: Type `<module 'asyncio'>` has no attribute `subprocess`
 --> test.py:3:7
  |
1 | import asyncio
2 |
3 | print(asyncio.subprocess.PIPE)
  |       ^^^^^^^^^^^^^^^^^^
  |
info: `unresolved-attribute` is enabled by default
```

Mypy and pyright seem to not throw any errors

### Version

_No response_

---

_Renamed from "Module not recognized" to "Module not recognized for `asyncio`" by @ion-elgreco on 2025-05-10 17:16_

---

_Closed by @AlexWaygood on 2025-05-10 17:26_

---
