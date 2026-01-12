```yaml
number: 1414
title: "Add support for `functools.total_ordering`"
type: issue
state: closed
author: sharkdp
labels:
  - runtime semantics
assignees: []
created_at: 2025-10-23T08:50:29Z
updated_at: 2025-10-23T08:52:16Z
url: https://github.com/astral-sh/ty/issues/1414
synced_at: 2026-01-12T15:54:25Z
```

# Add support for `functools.total_ordering`

---

_@sharkdp_

```py
from functools import total_ordering


@total_ordering
class C:
    def __eq__(self, other) -> bool:
        raise NotImplementedError

    def __lt__(self, other) -> bool:
        raise NotImplementedError


# This should not be an error:
C() >= C()
```
https://play.ty.dev/309a1765-a87b-465f-9e80-8919d3765ee0

---

_Label `runtime semantics` added by @sharkdp on 2025-10-23 08:50_

---

_Closed by @AlexWaygood on 2025-10-23 08:52_

---
