```yaml
number: 2288
title: "Respect signature of metaclass's `__call__`"
type: issue
state: open
author: taminomara
labels:
  - typing semantics
assignees: []
created_at: 2025-12-31T06:48:36Z
updated_at: 2026-01-08T18:12:16Z
url: https://github.com/astral-sh/ty/issues/2288
synced_at: 2026-01-12T15:54:26Z
```

# Respect signature of metaclass's `__call__`

---

_@taminomara_

Another feature request similar to #281: respect signature of `__call__` defined on a metaclass.

```python
class Meta(type):
    def __call__(cls, arg: int) -> str: ...

class T(metaclass=Meta):
    pass

# Inferred type: `T`, should be `str`.
t = T()  # Should report missing argument `arg`.
```

---

_Renamed from "Respect return type of metaclass's `__call__`" to "Respect signature of metaclass's `__call__`" by @taminomara on 2025-12-31 06:55_

---

_Label `typing semantics` added by @MichaReiser on 2025-12-31 08:17_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 08:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-31 16:39_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-08 18:12_

---

_Added to milestone `Stable` by @carljm on 2026-01-08 18:12_

---
