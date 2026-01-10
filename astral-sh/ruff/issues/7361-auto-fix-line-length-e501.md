---
number: 7361
title: Auto-fix line length (E501)
type: issue
state: closed
author: xaah
labels:
  - question
assignees: []
created_at: 2023-09-13T18:58:30Z
updated_at: 2023-09-13T19:10:26Z
url: https://github.com/astral-sh/ruff/issues/7361
synced_at: 2026-01-10T01:22:47Z
---

# Auto-fix line length (E501)

---

_Issue opened by @xaah on 2023-09-13 18:58_

Does auto-fix support E501? It doesn't seem to auto-fix line lengths in my setup, wondering if this is by design or a bug on my side.

---

_Comment by @zanieb on 2023-09-13 19:04_

E501 does not support auto-fix. I'd recommend using a formatter instead e.g. Black or #1904 

---

_Closed by @zanieb on 2023-09-13 19:04_

---

_Label `question` added by @zanieb on 2023-09-13 19:04_

---

_Comment by @xaah on 2023-09-13 19:10_

Ah ok thanks, I'll try using Black for now.

---

_Referenced in [astral-sh/ruff#7414](../../astral-sh/ruff/issues/7414.md) on 2023-09-15 18:16_

---
