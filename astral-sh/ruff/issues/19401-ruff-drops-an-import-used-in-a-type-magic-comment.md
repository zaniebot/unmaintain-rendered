---
number: 19401
title: "Ruff drops an import used in a `type: ` magic comment"
type: issue
state: closed
author: antonioan
labels: []
assignees: []
created_at: 2025-07-17T14:53:46Z
updated_at: 2025-07-17T16:04:09Z
url: https://github.com/astral-sh/ruff/issues/19401
synced_at: 2026-01-10T01:23:00Z
---

# Ruff drops an import used in a `type: ` magic comment

---

_Issue opened by @antonioan on 2025-07-17 14:53_

### Summary

The import from `typing` below is dropped by ruff, though it is used in the magic comment, read by mypy and PyCharm, among other type validators:

```python
from typing import Any

for k, v in {"a": "str", "b": 10}.items():  # type: str, Any
  print(k, v)
```

### Version

0.11.10

---

_Closed by @ntBre on 2025-07-17 16:04_

---
