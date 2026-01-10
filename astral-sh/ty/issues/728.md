```yaml
number: 728
title: Nonlocal names do not respect star-import reachability constraints
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - imports
  - control flow
assignees: []
created_at: 2025-06-30T14:51:40Z
updated_at: 2025-07-01T09:06:38Z
url: https://github.com/astral-sh/ty/issues/728
synced_at: 2026-01-10T02:07:36Z
```

# Nonlocal names do not respect star-import reachability constraints

---

_Issue opened by @sharkdp on 2025-06-30 14:51_

We currently don't record the special `*`-import reachability constraints for `UseDefMapBuilder::reachable_definitions`, only for `UseDefMapBuilder::place_states`.

`mod.py`

```py
class Internal: ...

__all__ = []
```

`main.py`

```py
from module import *

Internal  # unresolved-reference (as expected)

def _():
    Internal  # no error here!
```

https://play.ty.dev/91dab8ab-11cc-4e8c-9ac9-d3b1a94b7d20

---

_Label `bug` added by @sharkdp on 2025-06-30 14:51_

---

_Label `imports` added by @sharkdp on 2025-06-30 14:51_

---

_Label `control flow` added by @sharkdp on 2025-06-30 14:51_

---

_Assigned to @sharkdp by @sharkdp on 2025-06-30 14:51_

---

_Closed by @sharkdp on 2025-07-01 09:06_

---
