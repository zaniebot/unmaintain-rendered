```yaml
number: 17815
title: "Global is marked as \"used when not defined\""
type: issue
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
created_at: 2025-05-03T14:27:42Z
updated_at: 2025-05-03T14:36:33Z
url: https://github.com/astral-sh/ruff/issues/17815
synced_at: 2026-01-12T15:54:56Z
```

# Global is marked as "used when not defined"

---

_@charliermarsh_

```python
SINGLETON: int | None = None


def get_singleton() -> int:
    global SINGLETON

    if SINGLETON is not None:
        return SINGLETON
    
    SINGLETON = 1

    return SINGLETON
```

See: https://types.ruff.rs/474971dc-8790-40ab-9b9e-c1fa06f20a04

---

_Label `red-knot` added by @charliermarsh on 2025-05-03 14:27_

---

_Comment by @AlexWaygood on 2025-05-03 14:36_

Should be fixed soon by https://github.com/astral-sh/ruff/pull/17637!

---

_Closed by @AlexWaygood on 2025-05-03 14:36_

---
