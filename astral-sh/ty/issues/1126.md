```yaml
number: 1126
title: "Inconsistent semantic highlighting for `self`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
assignees: []
created_at: 2025-09-04T08:01:20Z
updated_at: 2025-10-20T17:32:50Z
url: https://github.com/astral-sh/ty/issues/1126
synced_at: 2026-01-10T02:06:25Z
```

# Inconsistent semantic highlighting for `self`

---

_Issue opened by @sharkdp on 2025-09-04 08:01_

### Summary

The two occurrences of `self` in the following snippet are highlighted in a different way:

```py
class C:
    def __init__(self):
        self.annotated: int = 1
        self.non_annotated = 1
```

<img width="534" height="161" alt="Image" src="https://github.com/user-attachments/assets/4835b865-536c-4d1e-8251-8ae8fb9c90f1" />

https://play.ty.dev/8575f32d-d5f6-4921-9c59-c9db93bf3dc8

### Version

1e34f3f20

---

_Label `bug` added by @sharkdp on 2025-09-04 08:01_

---

_Label `server` added by @sharkdp on 2025-09-04 08:01_

---

_Added to milestone `Beta` by @MichaReiser on 2025-09-15 08:32_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-10-17 15:18_

---

_Closed by @MichaReiser on 2025-10-20 17:32_

---
