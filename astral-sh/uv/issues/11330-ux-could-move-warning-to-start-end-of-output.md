---
number: 11330
title: "[UX] could move warning to start/end of output instead of middle?"
type: issue
state: open
author: T-256
labels:
  - question
assignees: []
created_at: 2025-02-07T20:46:47Z
updated_at: 2025-02-07T20:46:47Z
url: https://github.com/astral-sh/uv/issues/11330
synced_at: 2026-01-10T01:25:04Z
---

# [UX] could move warning to start/end of output instead of middle?

---

_Issue opened by @T-256 on 2025-02-07 20:46_

### Question

```
Resolved 19 packages in 1m 44s
Prepared 6 packages in 16.86s
Uninstalled 1 package in 12ms
░░░░░░░░░░░░░░░░░░░░ [0/7] Installing wheels...                                                                                       
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 7 packages in 678ms
 + altgraph==0.17.4
 + pefile==2024.8.26
 + pywin32-ctypes==0.2.3
 + setuptools==68.0.0
```

Also, I don't think the `░░░░░░░░░░░░░░░░░░░░ [0/7] Installing wheels...` should persist in final output. Does the warning cause it to be persisted?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @T-256 on 2025-02-07 20:46_

---
