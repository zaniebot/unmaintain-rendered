```yaml
number: 15268
title: "FURB171: Potentially unsafe fix"
type: issue
state: closed
author: fedyakov
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-01-05T14:19:46Z
updated_at: 2025-01-05T20:58:31Z
url: https://github.com/astral-sh/ruff/issues/15268
synced_at: 2026-01-12T15:54:54Z
```

# FURB171: Potentially unsafe fix

---

_@fedyakov_

FURB171 auto-fixes
```python
s in " "
```
into
```python
s == " "
```
But the Initial expression is equivalent to `s == "" or s == " "`, not `s == " "`.
It's a valid warning and safe fix for explicit collections like lists or sets, but probably should be disabled or at least moved to unsafe for strings.

---

_Label `bug` added by @AlexWaygood on 2025-01-05 15:05_

---

_Label `help wanted` added by @AlexWaygood on 2025-01-05 15:05_

---

_Closed by @AlexWaygood on 2025-01-05 17:54_

---
