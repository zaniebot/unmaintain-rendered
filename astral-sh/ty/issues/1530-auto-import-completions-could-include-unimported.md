```yaml
number: 1530
title: Auto-import completions could include unimported modules in more places
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-12T13:22:50Z
updated_at: 2025-12-04T22:37:39Z
url: https://github.com/astral-sh/ty/issues/1530
synced_at: 2026-01-12T15:54:25Z
```

# Auto-import completions could include unimported modules in more places

---

_@AlexWaygood_

In this situation, ty will offer to auto-import `my_module_that_could_be_imported` for me, which is awesome:

<img width="1320" height="656" alt="Image" src="https://github.com/user-attachments/assets/abf13d5a-f030-4b01-9494-cce3c709fa92" />

But in this situation, it will not, which feels a bit surprising:

<img width="1216" height="396" alt="Image" src="https://github.com/user-attachments/assets/e1e1837e-0efe-40b6-9509-6cb4944170b7" />

---

_Label `server` added by @AlexWaygood on 2025-11-12 13:22_

---

_Label `completions` added by @AlexWaygood on 2025-11-12 13:22_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:12_

---

_Comment by @BurntSushi on 2025-11-14 13:50_

Yeah I think auto-import currently just does not include modules as symbols themselves that can be imported. I _believe_ this should be pretty straight-forward to fix.

---

_Closed by @BurntSushi on 2025-12-04 22:37_

---
