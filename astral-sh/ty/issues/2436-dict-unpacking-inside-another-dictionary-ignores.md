```yaml
number: 2436
title: Dict unpacking inside another dictionary ignores annotations
type: issue
state: open
author: AndBoyS
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2026-01-10T13:40:07Z
updated_at: 2026-01-13T03:37:21Z
url: https://github.com/astral-sh/ty/issues/2436
synced_at: 2026-01-13T04:30:28Z
```

# Dict unpacking inside another dictionary ignores annotations

---

_@AndBoyS_

### Summary

```python
from typing_extensions import reveal_type
from typing import Any


kwargs: dict[str, Any] = {
    "a": "",
    "b": 1,
}
reveal_type(kwargs)  # dict[str, Any]

kwargs: dict[str, Any] = {
    **{"a": "", "b": 1},
}
reveal_type(kwargs)  # dict[str | Unknown, Any | str | int]
```

### Version

0.0.11

---

_Comment by @ibraheemdev on 2026-01-10 20:15_

Thanks for the report! This is related to https://github.com/astral-sh/ty/issues/1332.

---

_Label `bug` added by @ibraheemdev on 2026-01-10 20:15_

---

_Label `bidirectional inference` added by @ibraheemdev on 2026-01-10 20:15_

---

_Added to milestone `Stable` by @carljm on 2026-01-13 03:37_

---
