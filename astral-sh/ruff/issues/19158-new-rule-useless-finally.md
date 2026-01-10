---
number: 19158
title: "New rule: useless `finally`"
type: issue
state: open
author: harupy
labels:
  - rule
assignees: []
created_at: 2025-07-06T07:45:40Z
updated_at: 2025-07-07T14:28:11Z
url: https://github.com/astral-sh/ruff/issues/19158
synced_at: 2026-01-10T01:23:00Z
---

# New rule: useless `finally`

---

_Issue opened by @harupy on 2025-07-06 07:45_

### Summary

When `finally` only contains `pass`, it's useless:


```python
try:
    x = (1 / 0)
finally:
    pass  # <- useless finally
```

Playground: https://play.ruff.rs/db968565-8152-4cd5-b81b-2005fc0ce58a

---

_Renamed from "Useless `finally`" to "New rule: useless `finally`" by @harupy on 2025-07-06 07:46_

---

_Comment by @MichaReiser on 2025-07-07 14:28_

Related to https://github.com/astral-sh/ruff/issues/13929

---

_Label `rule` added by @MichaReiser on 2025-07-07 14:28_

---
