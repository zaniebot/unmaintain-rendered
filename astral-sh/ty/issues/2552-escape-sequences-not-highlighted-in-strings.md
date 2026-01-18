```yaml
number: 2552
title: escape sequences not highlighted in strings
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-18T11:04:58Z
updated_at: 2026-01-18T12:05:12Z
url: https://github.com/astral-sh/ty/issues/2552
synced_at: 2026-01-18T12:19:22Z
```

# escape sequences not highlighted in strings

---

_@MichaReiser_

### Summary

Escape sequences like `\n`, `\t`, `\\` are shown in the same color as regular string text. Pylance highlights them differently. 

First reported in https://github.com/astral-sh/ty/issues/2539

### Version

_No response_

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-18 11:04_

---

_Label `bug` added by @MichaReiser on 2026-01-18 11:04_

---

_Label `server` added by @MichaReiser on 2026-01-18 11:04_

---

_Comment by @MichaReiser on 2026-01-18 11:55_

This is an instance where our string literal classification hurts. Pylance, in comparison, doesn't emit a string literal classification, falling back to the textmate grammer's escape classication

---
