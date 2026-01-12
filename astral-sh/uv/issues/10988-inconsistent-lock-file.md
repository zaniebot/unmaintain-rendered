```yaml
number: 10988
title: Inconsistent lock file
type: issue
state: closed
author: xixixao
labels:
  - question
assignees: []
created_at: 2025-01-27T15:25:56Z
updated_at: 2025-01-28T09:04:11Z
url: https://github.com/astral-sh/uv/issues/10988
synced_at: 2026-01-12T16:00:25Z
```

# Inconsistent lock file

---

_@xixixao_

### Summary

I'm getting different lock file between different people and different runs on my machine, without changes to `pyproject.toml`s. We use workspaces. The diff:

```
version = 1
 requires-python = "==3.11.*"
 resolution-markers = [
-    "sys_platform != 'darwin' and sys_platform != 'win32'",
+    "platform_system == 'Windows' and sys_platform != 'darwin' and sys_platform != 'win32'",
+    "platform_system == 'Darwin' and sys_platform != 'darwin' and sys_platform != 'win32'",
+    "platform_system != 'Darwin' and platform_system != 'Windows' and sys_platform != 'darwin' and sys_platform != 'win32'",
     "sys_platform == 'win32'",
     "sys_platform == 'darwin'",
 ]
```

and also

```
 dependencies = [
-    { name = "colorama", marker = "sys_platform == 'win32'" },
+    { name = "colorama", marker = "platform_system == 'Windows' and sys_platform != 'darwin'" },
 ]
```

It's suspicious that this has to do with platform (we have one guy on Windows, rest is on macOS).

### Platform

macos and WSL

### Version

0.5.24

### Python version

3.11

---

_Label `bug` added by @xixixao on 2025-01-27 15:25_

---

_Comment by @charliermarsh on 2025-01-27 15:27_

Whoever is the `+` is using a different (older) uv version.

---

_Comment by @charliermarsh on 2025-01-27 15:27_

`platform_system == 'Windows'` can never appear in a lockfile on newer uv versions.

---

_Label `bug` removed by @charliermarsh on 2025-01-27 15:27_

---

_Label `question` added by @charliermarsh on 2025-01-27 15:27_

---

_Comment by @xixixao on 2025-01-28 09:04_

Thanks! That was the issue.

---

_Closed by @xixixao on 2025-01-28 09:04_

---
