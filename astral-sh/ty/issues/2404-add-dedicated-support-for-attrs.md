```yaml
number: 2404
title: add dedicated support for attrs
type: issue
state: open
author: carljm
labels:
  - library
assignees: []
created_at: 2026-01-08T18:23:54Z
updated_at: 2026-01-12T07:41:22Z
url: https://github.com/astral-sh/ty/issues/2404
synced_at: 2026-01-12T15:54:26Z
```

# add dedicated support for attrs

---

_@carljm_

_No description provided._

---

_Added to milestone `Stable` by @carljm on 2026-01-08 18:23_

---

_Label `library` added by @carljm on 2026-01-08 18:23_

---

_Comment by @hynek on 2026-01-12 07:41_

wow I came begging for one particular feature and found this late xmas present! 😻

LMK if you need any help/advice!

---

As a starter (and the reason I came here): could y'all implement attrs's underscore prefix stripping? 😇

I.e. defining an attribute called `_foo` creates an `__init__(self, foo)` because there's no such thing as a private argument and you wouldn't call your argument that yourself either (this goes back to attrs's philosophy of helping you to write classes the way _you_ would write them). This is currently by far my biggest annoyance with ty's LSP server.

Should I open an subissue (hilariously I've never used that feature so not sure how)?

---
