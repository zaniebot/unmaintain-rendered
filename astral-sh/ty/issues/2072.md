```yaml
number: 2072
title: "Fix `Todo` type inferred when subscripting an intersection"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-18T14:18:07Z
updated_at: 2025-12-18T20:51:01Z
url: https://github.com/astral-sh/ty/issues/2072
synced_at: 2026-01-10T01:54:00Z
```

# Fix `Todo` type inferred when subscripting an intersection

---

_Issue opened by @AlexWaygood on 2025-12-18 14:18_

```py
from typing import Sequence

class C: ...

def f(x: Sequence[int]):
    if isinstance(x, C):
        reveal_type(x[0])  # revealed: @Todo
```

We should infer `int` there (and, same as our current behaviour, we should not emit a diagnostic).

This was previously attempted in https://github.com/astral-sh/ruff/pull/18846 but then reverted in https://github.com/astral-sh/ruff/pull/18920

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-18 14:18_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-18 14:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-18 20:51_

---
