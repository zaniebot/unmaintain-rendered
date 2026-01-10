```yaml
number: 1621
title: "Zed: No LSP features for unsaved buffers"
type: issue
state: open
author: MichaReiser
labels:
  - editor
assignees: []
created_at: 2025-11-24T17:12:20Z
updated_at: 2025-12-31T15:36:05Z
url: https://github.com/astral-sh/ty/issues/1621
synced_at: 2026-01-10T01:56:40Z
```

# Zed: No LSP features for unsaved buffers

---

_Issue opened by @MichaReiser on 2025-11-24 17:12_

ty doesn't provide any type checking or LSP functionality for unsaved files (buffers):

* Create a new unnamed file
* Change the language to Python
* Paste: `sys.version`
* Note how there's no diagnostic for `sys` being undefined

I think this is related to the following upstream issues:

* https://github.com/zed-industries/zed/issues/17098
* https://github.com/zed-industries/zed/issues/20839

because I don't see Zed sending any `didOpen` notification to ty's (or Ruff's) LSP. 

I commented on the upstream issue to hear from the Zed folks whether it's indeed an upstream issue and if they're aware of it that impacts any LSP.



---

_Label `server` added by @MichaReiser on 2025-11-24 17:12_

---

_Label `server` removed by @MichaReiser on 2025-12-04 13:54_

---

_Label `editor` added by @MichaReiser on 2025-12-04 13:54_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:36_

---
