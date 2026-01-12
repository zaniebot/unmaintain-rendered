```yaml
number: 738
title: "ty panics on input `[.`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-07-01T12:36:38Z
updated_at: 2025-07-09T06:46:35Z
url: https://github.com/astral-sh/ty/issues/738
synced_at: 2026-01-12T15:54:23Z
```

# ty panics on input `[.`

---

_@sharkdp_

### Summary

ty currently panics on this invalid-syntax input
```py
[.
```
The original example was slightly larger and came up while editing something in the playground.

### Version

ty 0.0.1-alpha.12

---

_Assigned to @sharkdp by @sharkdp on 2025-07-01 12:36_

---

_Label `bug` added by @sharkdp on 2025-07-01 12:36_

---

_Label `fatal` added by @sharkdp on 2025-07-01 12:36_

---

_Closed by @sharkdp on 2025-07-09 06:46_

---
