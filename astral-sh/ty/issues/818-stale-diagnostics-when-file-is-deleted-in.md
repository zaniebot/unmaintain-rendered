```yaml
number: 818
title: Stale diagnostics when file is deleted in workspace diagnostic mode
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2025-07-12T03:35:39Z
updated_at: 2025-08-04T10:26:40Z
url: https://github.com/astral-sh/ty/issues/818
synced_at: 2026-01-10T02:06:24Z
```

# Stale diagnostics when file is deleted in workspace diagnostic mode

---

_Issue opened by @dhruvmanila on 2025-07-12 03:35_

### Summary

For the following setting:
```json
{
    "ty.diagnosticMode": "workspace"
}
```

If a file containing one or more diagnostics is deleted, then the diagnostics are still visible in the "Problems" panel:

https://github.com/user-attachments/assets/8210fde9-d4d5-40be-b16b-8befadd76040

### Version

ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)

---

_Label `bug` added by @dhruvmanila on 2025-07-12 03:35_

---

_Label `server` added by @dhruvmanila on 2025-07-12 03:35_

---

_Renamed from "[ty] Stale diagnostics when file is deleted in workspace diagnostic mode" to "Stale diagnostics when file is deleted in workspace diagnostic mode" by @dhruvmanila on 2025-07-15 06:25_

---

_Closed by @MichaReiser on 2025-08-04 10:26_

---
