---
number: 18011
title: "Ignore `no-explicit-stacklevel`/B028 if `skip_file_prefixes` is used"
type: issue
state: closed
author: apple1417
labels:
  - bug
  - rule
assignees: []
created_at: 2025-05-11T00:53:50Z
updated_at: 2025-05-12T22:06:52Z
url: https://github.com/astral-sh/ruff/issues/18011
synced_at: 2026-01-07T13:12:16-06:00
---

# Ignore `no-explicit-stacklevel`/B028 if `skip_file_prefixes` is used

---

_Issue opened by @apple1417 on 2025-05-11 00:53_

### Summary

Setting `skip_file_prefixes` serves the same purpose as `stacklevel`, making sure the warning fires in a more informative place. It also actually implicitly sets stack level to at least 2.
> When prefixes are supplied, stacklevel is implicitly overridden to be `max(2, stacklevel)`.

This change was already made upstream https://github.com/PyCQA/flake8-bugbear/issues/497

Sample code:
```py
import os
import warnings

warnings.warn("test", skip_file_prefixes=(os.path.dirname(__file__),))
```
- No explicit `stacklevel` keyword argument found (B028) [Ln 4, Col 1]

https://play.ruff.rs/f3bb6bb2-4d66-4eea-b86e-fa1c54c98f6b

---

_Label `bug` added by @dylwil3 on 2025-05-11 20:38_

---

_Label `rule` added by @dylwil3 on 2025-05-11 20:38_

---

_Referenced in [astral-sh/ruff#18047](../../astral-sh/ruff/pulls/18047.md) on 2025-05-12 14:45_

---

_Closed by @dylwil3 on 2025-05-12 22:06_

---

_Closed by @dylwil3 on 2025-05-12 22:06_

---
