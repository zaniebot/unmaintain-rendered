```yaml
number: 2276
title: "Server: auto-indent works much worse when Pylance is disabled"
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2025-12-30T13:00:22Z
updated_at: 2026-01-05T15:19:56Z
url: https://github.com/astral-sh/ty/issues/2276
synced_at: 2026-01-12T15:54:26Z
```

# Server: auto-indent works much worse when Pylance is disabled

---

_@AlexWaygood_

### Summary

Consider the below video. With Pylance enabled (at the beginning of the video), hitting return automatically indents my cursor by 8 spaces, which is where I want it to be to add a new line to the beginning of my function. But with Pylance disabled (so that ty is the only Python LSP active), hitting return only automatically indents my cursor by 4 spaces

https://github.com/user-attachments/assets/d021e9d4-db44-441c-9423-63847d72e967

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-12-30 13:00_

---

_Comment by @MichaReiser on 2025-12-30 13:40_

Yeah, ty doesn't do any auto-indent at all

---

_Comment by @MichaReiser on 2025-12-30 13:42_

I'm also not sure if this is something that needs to be implemented in VS Code. At least, it's not obvious to me what LSP feature this corresponds to. Maybe format on type? https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_onTypeFormatting

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:46_

---

_Comment by @Gankra on 2026-01-05 15:19_

It's either that or like... semantic-tokens(???)

---
