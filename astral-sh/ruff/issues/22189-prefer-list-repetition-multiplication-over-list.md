```yaml
number: 22189
title: Prefer list repetition/multiplication over list comprehension
type: issue
state: open
author: injust
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-25T06:32:29Z
updated_at: 2025-12-25T14:43:00Z
url: https://github.com/astral-sh/ruff/issues/22189
synced_at: 2026-01-10T11:10:00Z
```

# Prefer list repetition/multiplication over list comprehension

---

_Issue opened by @injust on 2025-12-25 06:32_

### Summary

List repetition/multiplication (`[foo] * n`) is (IMO) easier to read and more Pythonic than a list comprehension (`[foo for _ in range(n)]`).

I haven't tested performance, but from a stylistic perspective, I think the former should be preferred.

---

_Renamed from "Prefer list repetition/multiplication (`[foo] * n`) over `[foo for _ in range(n)]`" to "Prefer list repetition/multiplication (`[foo] * n`) over list comprehension (`[foo for _ in range(n)]`)" by @injust on 2025-12-25 06:32_

---

_Renamed from "Prefer list repetition/multiplication (`[foo] * n`) over list comprehension (`[foo for _ in range(n)]`)" to "Prefer list repetition/multiplication over list comprehension" by @injust on 2025-12-25 06:33_

---

_Label `rule` added by @ntBre on 2025-12-25 14:43_

---

_Label `needs-decision` added by @ntBre on 2025-12-25 14:43_

---
