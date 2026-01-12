```yaml
number: 1769
title: Incorrect handling of dataclass kw_only with inheritance
type: issue
state: closed
author: Matt-Ord
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-12-05T12:46:00Z
updated_at: 2025-12-10T09:12:20Z
url: https://github.com/astral-sh/ty/issues/1769
synced_at: 2026-01-12T15:54:25Z
```

# Incorrect handling of dataclass kw_only with inheritance

---

_@Matt-Ord_

### Summary

```py
from dataclasses import dataclass

@dataclass
class Inner:
    inner: int


@dataclass(kw_only=True)
class Outer(Inner):
    outer: int


_ = Outer(0, outer=5)
```

Should pass, but fails with missing-argument (passes in pyright)

### Version

ty 0.0.1-alpha.31

---

_Label `dataclasses` added by @AlexWaygood on 2025-12-05 12:50_

---

_Label `bug` added by @AlexWaygood on 2025-12-05 12:51_

---

_Added to milestone `Stable` by @carljm on 2025-12-05 19:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-06 03:04_

---

_Closed by @sharkdp on 2025-12-10 09:12_

---
