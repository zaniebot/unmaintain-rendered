---
number: 1720
title: Cache/parallelize find references
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - server
assignees: []
created_at: 2025-12-02T13:54:46Z
updated_at: 2025-12-31T15:37:59Z
url: https://github.com/astral-sh/ty/issues/1720
synced_at: 2026-01-10T01:51:14Z
---

# Cache/parallelize find references

---

_Issue opened by @MichaReiser on 2025-12-02 13:54_

Finding common references, e.g. like finding all references for `asyncio` can be slow in a large project like homeassistant. We should speed that up by:

* Can we cache some intermediate steps so that subsequent find references are faster
* Use multithreading

Ideally, I think we would do both

---

_Label `performance` added by @MichaReiser on 2025-12-02 13:54_

---

_Label `server` added by @MichaReiser on 2025-12-02 13:54_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:37_

---
