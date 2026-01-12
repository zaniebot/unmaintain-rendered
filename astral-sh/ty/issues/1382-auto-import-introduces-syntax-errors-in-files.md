```yaml
number: 1382
title: "Auto import introduces syntax errors in files with `from __future__ import annotations`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-10-17T10:38:21Z
updated_at: 2025-10-21T06:14:41Z
url: https://github.com/astral-sh/ty/issues/1382
synced_at: 2026-01-12T15:54:25Z
```

# Auto import introduces syntax errors in files with `from __future__ import annotations`

---

_@sharkdp_

### Summary

Auto import adds new import statements at the beginning of the file. This breaks possible `from __future__ import annotations` statements which need to be at the top.

Before (https://play.ty.dev/756bd987-60fd-494c-9d1a-9270493f064d):

<img width="640" height="254" alt="Image" src="https://github.com/user-attachments/assets/116be1ee-4e94-4e8e-9a7a-bd8d1fa99720" />

After:

<img width="407" height="152" alt="Image" src="https://github.com/user-attachments/assets/a8b9d5fb-f542-4721-89af-b3fe43b0d374" />

### Version

a21cde8a5

---

_Label `server` added by @sharkdp on 2025-10-17 10:38_

---

_Label `completions` added by @sharkdp on 2025-10-17 10:38_

---

_Label `bug` added by @AlexWaygood on 2025-10-17 10:40_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-10-17 10:40_

---

_Renamed from "Auto import breaks `from __future__ import annotations`" to "Auto import introduces syntax errors in files with `from __future__ import annotations`" by @AlexWaygood on 2025-10-17 10:40_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-10-17 15:18_

---

_Comment by @MichaReiser on 2025-10-20 09:27_

We probably want this in ty

https://github.com/astral-sh/ruff/blob/9a29f7a339291868a03374c146f60f7e9172056e/crates/ruff_linter/src/importer/mod.rs#L535-L545

---

_Closed by @MichaReiser on 2025-10-21 06:14_

---
