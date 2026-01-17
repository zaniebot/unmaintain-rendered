```yaml
number: 1519
title: add a diagnostic for NewTypes with TypeVars in their base type
type: issue
state: closed
author: oconnor663
labels:
  - typing semantics
assignees: []
created_at: 2025-11-11T00:42:19Z
updated_at: 2026-01-17T15:23:54Z
url: https://github.com/astral-sh/ty/issues/1519
synced_at: 2026-01-17T16:15:00Z
```

# add a diagnostic for NewTypes with TypeVars in their base type

---

_@oconnor663_

TODO from: https://github.com/astral-sh/ruff/pull/21157#pullrequestreview-3445518919

Something like this should be an error:

```py
T = TypeVar("T")
N = NewType("N", list[T])  # currently accepted, but not a "proper class"
```

---

_Label `typing semantics` added by @oconnor663 on 2025-11-11 00:42_

---

_Added to milestone `GA` by @AlexWaygood on 2025-11-11 00:44_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-17 15:12_

---

_Closed by @AlexWaygood on 2026-01-17 15:23_

---
