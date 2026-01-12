```yaml
number: 14780
title: "(🎁) new rule: useless f-string"
type: issue
state: closed
author: KotlinIsland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-05T00:22:26Z
updated_at: 2024-12-05T03:59:59Z
url: https://github.com/astral-sh/ruff/issues/14780
synced_at: 2026-01-12T15:54:54Z
```

# (🎁) new rule: useless f-string

---

_@KotlinIsland_

i see code like this:
```py
a = f"{b}"
```
pointless imo, just use `str(b)` or `format(b)`

---

_Label `rule` added by @AlexWaygood on 2024-12-05 01:15_

---

_Label `needs-decision` added by @AlexWaygood on 2024-12-05 01:15_

---

_Comment by @dhruvmanila on 2024-12-05 03:59_

Duplicate of https://github.com/astral-sh/ruff/issues/10945.

---

_Closed by @dhruvmanila on 2024-12-05 03:59_

---
