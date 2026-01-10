```yaml
number: 1221
title: generic aliases should be understood as what they are aliases to
type: issue
state: open
author: KotlinIsland
labels:
  - wish
  - runtime semantics
assignees: []
created_at: 2025-09-20T15:18:45Z
updated_at: 2025-11-18T16:10:38Z
url: https://github.com/astral-sh/ty/issues/1221
synced_at: 2026-01-10T01:58:59Z
```

# generic aliases should be understood as what they are aliases to

---

_Issue opened by @KotlinIsland on 2025-09-20 15:18_

### Summary

```py
from typing import Callable, List

print(Callable.mro()) # ty: no idea, works at runtime
print(List.mro())  # ty: no idea, works at runtime
```

### Version

_No response_

---

_Label `wish` added by @AlexWaygood on 2025-09-22 08:04_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-09-22 08:04_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
