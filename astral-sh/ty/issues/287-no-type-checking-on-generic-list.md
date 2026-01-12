```yaml
number: 287
title: No type checking on generic list
type: issue
state: closed
author: ultrageopro
labels:
  - bug
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-05-09T02:58:11Z
updated_at: 2025-05-29T23:58:58Z
url: https://github.com/astral-sh/ty/issues/287
synced_at: 2026-01-12T15:54:22Z
```

# No type checking on generic list

---

_@ultrageopro_

### Summary

I've tried `ty check` and `ty check main.py`

![Image](https://github.com/user-attachments/assets/15227987-c93b-4438-a2c9-02aef3302914)

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Label `bug` added by @carljm on 2025-05-09 03:22_

---

_Label `generics` added by @carljm on 2025-05-09 03:22_

---

_Comment by @carljm on 2025-05-09 03:23_

Thanks for the report! Yes, this is a known issue where we don't properly understand some common standard library generic types which inherit from generic protocols. There is a PR open already to fix it, we are just working on eliminating some issues with that PR before we land it.

(Note for future: it's always better to include text code samples in bug reports, rather than screenshots.)

---

_Renamed from "No case-specific type checking" to "No type checking on generic list" by @carljm on 2025-05-09 03:24_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-05-11 11:02_

---

_Comment by @carljm on 2025-05-29 23:58_

This is partly fixed; we now do support these generic types, and support checking calls to methods of them.

What we are still missing here is captured by https://github.com/astral-sh/ty/issues/543 -- closing in favor of that.

---

_Closed by @carljm on 2025-05-29 23:58_

---
