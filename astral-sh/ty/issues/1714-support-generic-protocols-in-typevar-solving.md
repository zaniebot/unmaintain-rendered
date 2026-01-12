```yaml
number: 1714
title: support generic protocols in typevar solving
type: issue
state: open
author: carljm
labels:
  - generics
  - Protocols
assignees: []
created_at: 2025-12-01T23:45:54Z
updated_at: 2025-12-28T01:56:27Z
url: https://github.com/astral-sh/ty/issues/1714
synced_at: 2026-01-12T15:54:25Z
```

# support generic protocols in typevar solving

---

_@carljm_

_No description provided._

---

_Added to milestone `Beta` by @carljm on 2025-12-01 23:45_

---

_Label `generics` added by @carljm on 2025-12-01 23:45_

---

_Label `Protocols` added by @carljm on 2025-12-01 23:45_

---

_Assigned to @dcreager by @carljm on 2025-12-01 23:46_

---

_Assigned to @AlexWaygood by @carljm on 2025-12-01 23:46_

---

_Removed from milestone `Beta` by @carljm on 2025-12-16 21:10_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 21:10_

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Comment by @ooopus on 2025-12-28 01:56_

I think this is another instance of #1714 (generic protocol involvement in typevar solving).

When sorting a `list[str]` with `key=len`, ty widens the element type to `Sized`, which then causes false positives when the result is used as `str`.
```python
from __future__ import annotations

import re

def build_regex(forms: list[str]) -> str:
    sorted_forms = sorted(forms, key=len)

    reveal_type(sorted_forms)  # ty infers list[Sized] (unexpected)
    # Any downstream use that requires `str` then fails:
    escaped = [re.escape(f) for f in sorted_forms]  # ty: expected str/bytes, got Sized
    return "|".join(escaped)
```
Expected:
- `sorted_forms` should be inferred as `list[str]` (same element type as the input iterable).

Actual:
- `sorted_forms` is inferred as `list[Sized]`, losing the concrete element type and producing false positives.

---
