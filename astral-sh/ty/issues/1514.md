```yaml
number: 1514
title: Omit inlay hints for function argument names when the input matches it
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-11-10T14:11:00Z
updated_at: 2025-11-10T16:42:14Z
url: https://github.com/astral-sh/ty/issues/1514
synced_at: 2026-01-10T02:06:25Z
```

# Omit inlay hints for function argument names when the input matches it

---

_Issue opened by @Gankra on 2025-11-10 14:11_

An IMO very nice feature of rust-analyzer is that it omits inlay hints of names when the thing being passed to the name matches:

<img width="366" height="155" alt="Image" src="https://github.com/user-attachments/assets/4480ea92-01fe-4ba0-b514-b8fba5325ca9" />

ty has no such mercy:

<img width="614" height="31" alt="Image" src="https://github.com/user-attachments/assets/fa824697-c3a8-46e3-af24-458f7842bbc3" />

I've found this sort of thing to be a *surprisingly* effective feedback mechanism for catching mistakes. If I see a call riddled with inlay hints for arguments it stands out that I might have mixed up arguments! Also in general it encourages my codebases to have consistent names for the same thing, which is a nice little virtuous nudge.

---

_Assigned to @Gankra by @Gankra on 2025-11-10 14:11_

---

_Label `server` added by @Gankra on 2025-11-10 14:11_

---

_Added to milestone `GA` by @Gankra on 2025-11-10 14:11_

---

_Comment by @MichaReiser on 2025-11-10 15:21_

Agree, this is mostly noise :)

This should be a very easy win

---

_Closed by @Gankra on 2025-11-10 16:42_

---
