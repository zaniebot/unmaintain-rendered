```yaml
number: 11438
title: Improve error message for package name parsing when paths are accepted
type: issue
state: open
author: dimaqq
labels:
  - error messages
assignees: []
created_at: 2025-02-12T08:36:19Z
updated_at: 2025-02-12T14:59:17Z
url: https://github.com/astral-sh/uv/issues/11438
synced_at: 2026-01-12T16:00:36Z
```

# Improve error message for package name parsing when paths are accepted

---

_@dimaqq_

```sh
> uv pip install -e [tracing,testing] -U
error: Failed to parse: `[tracing,testing]`
  Caused by: Expected package name starting with an alphanumeric character, found `[`
[tracing,testing]
^
```

Yet, `.[xxx]` is allowed, and `.` is not alphanumeric.

---

_Label `error messages` added by @zanieb on 2025-02-12 14:58_

---

_Renamed from "Slightly misleading error text" to "Improve error message for package name parsing when paths are accepted" by @zanieb on 2025-02-12 14:59_

---
