```yaml
number: 925
title: Stale diagnostics
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-01T14:28:45Z
updated_at: 2025-08-04T10:19:19Z
url: https://github.com/astral-sh/ty/issues/925
synced_at: 2026-01-12T15:54:24Z
```

# Stale diagnostics

---

_@MichaReiser_

### Summary

* Open file A with no diagnostics. It imports a symbol from file b
* Change file B using `nano` (outside vs code) so that the import in file A now fails
* The server correctly returns new workspace diagnostics
* But VS code ignores them because the version of file a hasn't changed and pull diagnostics have precedence over workspace diagnostics.

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-08-01 14:28_

---

_Label `server` added by @MichaReiser on 2025-08-01 14:28_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-08-01 14:28_

---

_Closed by @MichaReiser on 2025-08-04 10:19_

---
