```yaml
number: 140
title: On-hover improvements
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-04-02T16:02:00Z
updated_at: 2025-08-15T14:43:50Z
url: https://github.com/astral-sh/ty/issues/140
synced_at: 2026-01-12T15:54:22Z
```

# On-hover improvements

---

_@MichaReiser_

Our first implementation of "On hover" only shows the inferred type (without highlighting) at the hovered position. This is nice, but we should surface more information:

* [ ] For symbols, e.g. `a` (`a: str`) or `foo.bar` (`bar: str`) show the symbol's signature (or `Test(a: str, b: int)` for a class). See pyright for examples. This requires a new API, similar to *Goto definition*
* [ ] Extract the docstring for the symbol and surface it as markdown.

https://github.com/user-attachments/assets/bd45faa9-218f-4357-b277-ba320b176acf


---

_Renamed from "[red-knot] On-hover improvements" to "On-hover improvements" by @MichaReiser on 2025-05-07 15:25_

---

_Label `server` added by @MichaReiser on 2025-05-08 08:06_

---

_Assigned to @Gankra by @Gankra on 2025-08-13 16:45_

---

_Comment by @MichaReiser on 2025-08-15 11:59_

I think we can close this one and track other improvements more granularly. What you think @Gankra? (at least the two bullet items are now implemented, at least to some extend)

---

_Closed by @carljm on 2025-08-15 14:43_

---
