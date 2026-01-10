```yaml
number: 1366
title: "Dataclass `field`s do not necessarily have a default value"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-10-16T09:02:16Z
updated_at: 2025-10-16T10:49:25Z
url: https://github.com/astral-sh/ty/issues/1366
synced_at: 2026-01-10T02:06:25Z
```

# Dataclass `field`s do not necessarily have a default value

---

_Issue opened by @sharkdp on 2025-10-16 09:02_

### Summary

This currently type-checks without any errors, but should not

```py
from dataclasses import dataclass, field

@dataclass
class A:
    a: str = field(kw_only=True)

A()
```

### Version

_No response_

---

_Label `bug` added by @sharkdp on 2025-10-16 09:02_

---

_Label `dataclasses` added by @sharkdp on 2025-10-16 09:02_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-16 09:02_

---

_Closed by @sharkdp on 2025-10-16 10:49_

---
