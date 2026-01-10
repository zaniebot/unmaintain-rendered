```yaml
number: 15740
title: "Warn when `tool.uv.build-backend` is set but wrong `[build-system]` is configured"
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-09-08T17:44:56Z
updated_at: 2025-09-09T12:32:41Z
url: https://github.com/astral-sh/uv/issues/15740
synced_at: 2026-01-10T03:23:54Z
```

# Warn when `tool.uv.build-backend` is set but wrong `[build-system]` is configured

---

_Issue opened by @zanieb on 2025-09-08 17:44_

e.g., in https://github.com/astral-sh/uv/issues/15655#issue-3379565490 we don't respect the `build-backend` setting because our build backend isn't being used. We should have warnings for

1. When no build system is configured
2. When the wrong build system is configured
3. When setuptools legacy would be used  (e.g., due to `package = true`)

---

_Label `error messages` added by @zanieb on 2025-09-08 17:45_

---

_Comment by @zanieb on 2025-09-08 17:45_

cc @konstin 

---

_Assigned to @konstin by @konstin on 2025-09-09 12:32_

---
