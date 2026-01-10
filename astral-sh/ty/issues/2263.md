---
number: 2263
title: Hide inlay hints on explicitly specified generic constructor assignments
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2025-12-29T19:49:29Z
updated_at: 2025-12-31T15:46:23Z
url: https://github.com/astral-sh/ty/issues/2263
synced_at: 2026-01-10T01:51:14Z
---

# Hide inlay hints on explicitly specified generic constructor assignments

---

_Issue opened by @MeGaGiGaGon on 2025-12-29 19:49_

Not sure how common of a pattern this is, but especially whenever I need an empty set I'll write it like this:
```py
x = set[int]()
```
Since otherwise with a type hint `set` would end up repeated twice.

But currently that makes an inlay hint with that duplicate information I was trying to avoid:
https://play.ty.dev/d13ad5a0-33b3-43f4-815a-25ac39528bc2

<img width="209" height="32" alt="Image" src="https://github.com/user-attachments/assets/6eb7687e-8dde-4776-9e31-593bbd790a88" />

It would be nice if in this case of the direct assignment of a generic-specified class constructor ty hid the inlay hint. Not sure how feasible that is to do.

---

_Label `server` added by @carljm on 2025-12-29 19:50_

---

_Comment by @carljm on 2025-12-29 19:50_

This seems like a pretty simple extension of our existing avoid-trivial-inlays logic?

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:46_

---
