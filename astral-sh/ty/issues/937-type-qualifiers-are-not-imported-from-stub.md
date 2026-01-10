```yaml
number: 937
title: Type qualifiers are not imported from stub declarations without a right hand side
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - stubs
assignees: []
created_at: 2025-08-05T08:34:05Z
updated_at: 2025-08-05T10:07:06Z
url: https://github.com/astral-sh/ty/issues/937
synced_at: 2026-01-10T02:06:24Z
```

# Type qualifiers are not imported from stub declarations without a right hand side

---

_Issue opened by @sharkdp on 2025-08-05 08:34_

### Summary

To reproduce:

* Open https://play.ty.dev/2b575126-a2e6-4e59-80d7-9409716d6700 and observe that we see a `ClassVar`-related diagnostic (correct)
* Rename `module.py` to `module.pyi` and observe that the error goes away (not correct)

### Version

main

---

_Label `bug` added by @sharkdp on 2025-08-05 08:34_

---

_Label `stubs` added by @sharkdp on 2025-08-05 08:34_

---

_Assigned to @sharkdp by @sharkdp on 2025-08-05 08:35_

---

_Closed by @sharkdp on 2025-08-05 10:07_

---
