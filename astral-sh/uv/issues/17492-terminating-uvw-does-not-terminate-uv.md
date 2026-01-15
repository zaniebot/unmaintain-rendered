```yaml
number: 17492
title: "Terminating `uvw` does not terminate `uv`"
type: issue
state: open
author: ouroborus
labels:
  - bug
assignees: []
created_at: 2026-01-15T18:04:57Z
updated_at: 2026-01-15T18:04:57Z
url: https://github.com/astral-sh/uv/issues/17492
synced_at: 2026-01-15T18:49:38Z
```

# Terminating `uvw` does not terminate `uv`

---

_@ouroborus_

### Summary

When `uvw.exe` is terminated, the child process `uv.exe` does not end.

For me, this means that Task Scheduler cannot terminate the Python script as it just terminates `uvw.exe`.

### Platform

Windows

### Version

uv 0.9.21 (0dc9556ad 2025-12-30)

### Python version

_No response_

---

_Label `bug` added by @ouroborus on 2026-01-15 18:04_

---
