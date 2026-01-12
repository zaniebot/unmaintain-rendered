```yaml
number: 72
title: Revisit file watching in server
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-05-07T12:50:25Z
updated_at: 2025-08-22T12:40:13Z
url: https://github.com/astral-sh/ty/issues/72
synced_at: 2026-01-12T15:54:22Z
```

# Revisit file watching in server

---

_@MichaReiser_

https://github.com/astral-sh/ruff/pull/17912 added very basic file watching to the LSP. The main limitation is that it only watches project files. Files outside the project aren't watched. 



---

_Label `server` added by @MichaReiser on 2025-05-07 12:50_

---

_Renamed from "[ty] Revisit file watching in server" to "Revisit file watching in server" by @MichaReiser on 2025-05-07 15:12_

---

_Added to milestone `beta` by @MichaReiser on 2025-05-07 15:59_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-05-28 08:58_

---

_Added to milestone `GA` by @MichaReiser on 2025-05-28 08:58_

---

_Comment by @MichaReiser on 2025-08-22 12:40_

I think we can close this now. @BurntSushi significantly improved the file watching infrastructure in the LSP.

---

_Closed by @MichaReiser on 2025-08-22 12:40_

---
