```yaml
number: 1641
title: Can Ty enforce static typing?
type: issue
state: closed
author: StandingPadAnimations
labels:
  - question
assignees: []
created_at: 2025-11-26T05:15:26Z
updated_at: 2025-11-26T08:04:00Z
url: https://github.com/astral-sh/ty/issues/1641
synced_at: 2026-01-10T01:58:59Z
```

# Can Ty enforce static typing?

---

_Issue opened by @StandingPadAnimations on 2025-11-26 05:15_

### Question

In Mypy, we can get something similar to static type enforcement with the following config:

```ini
[mypy]
disallow_untyped_defs = True
disallow_any_expr = True
```

Where all function definitions have to be typed, and expressions can't have the type `Any`. Can Ty do something similar?

### Version

_No response_

---

_Label `question` added by @StandingPadAnimations on 2025-11-26 05:15_

---

_Comment by @sharkdp on 2025-11-26 08:04_

> Where all function definitions have to be typed, and expressions can't have the type `Any`. Can Ty do something similar?

Not yet, please see https://github.com/astral-sh/ty/issues/527 and https://github.com/astral-sh/ty/issues/1240 for similar discussions.

---

_Closed by @sharkdp on 2025-11-26 08:04_

---
