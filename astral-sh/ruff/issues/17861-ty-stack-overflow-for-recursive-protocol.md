```yaml
number: 17861
title: "[ty] Stack overflow for recursive protocol definitions"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-05-05T14:04:59Z
updated_at: 2025-05-07T06:55:23Z
url: https://github.com/astral-sh/ruff/issues/17861
synced_at: 2026-01-10T11:09:58Z
```

# [ty] Stack overflow for recursive protocol definitions

---

_Issue opened by @sharkdp on 2025-05-05 14:04_

The following example leads to a stack overflow:
```py
from typing import Protocol

class P(Protocol):
    other: "P" | None
```

---

_Added to milestone `Red Knot Alpha` by @sharkdp on 2025-05-05 14:05_

---

_Label `bug` added by @sharkdp on 2025-05-05 14:05_

---

_Label `ty` added by @sharkdp on 2025-05-05 14:05_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-05 14:05_

---

_Closed by @sharkdp on 2025-05-07 06:55_

---

_Closed by @sharkdp on 2025-05-07 06:55_

---
