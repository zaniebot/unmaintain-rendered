```yaml
number: 2474
title: implement attrs underscore-prefix stripping
type: issue
state: open
author: carljm
labels:
  - library
assignees: []
created_at: 2026-01-13T02:10:19Z
updated_at: 2026-01-13T02:12:06Z
url: https://github.com/astral-sh/ty/issues/2474
synced_at: 2026-01-13T02:20:57Z
```

# implement attrs underscore-prefix stripping

---

_@carljm_

> could y'all implement attrs's underscore prefix stripping? 😇
> 
> I.e. defining an attribute called `_foo` creates an `__init__(self, foo)` because there's no such thing as a private argument and you wouldn't call your argument that yourself either (this goes back to attrs's philosophy of helping you to write classes the way _you_ would write them). This is currently by far my biggest annoyance with ty's LSP server.

 _Originally posted by @hynek in [#2404](https://github.com/astral-sh/ty/issues/2404#issuecomment-3737240578)_

---

_Label `library` added by @carljm on 2026-01-13 02:10_

---

_Added to milestone `Stable` by @carljm on 2026-01-13 02:12_

---
