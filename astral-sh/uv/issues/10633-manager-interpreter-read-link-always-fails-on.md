```yaml
number: 10633
title: Manager interpreter read link always fails on Windows
type: issue
state: closed
author: konstin
labels:
  - bug
  - windows
assignees: []
created_at: 2025-01-15T13:26:23Z
updated_at: 2025-01-15T20:17:09Z
url: https://github.com/astral-sh/uv/issues/10633
synced_at: 2026-01-12T16:00:17Z
```

# Manager interpreter read link always fails on Windows

---

_@konstin_

The following check always fails on Windows, since Windows Pythons are trampolines not links. On Windows, we should directly try to read the launcher.

https://github.com/astral-sh/uv/blob/80ac8db7db5b33a0bc46d2c0d722b704baed9c53/crates/uv/src/commands/python/install.rs#L363-L367

Example:

```
$ uv python install --preview 3.13 -v                
DEBUG uv 0.5.18+38 (df6e155fb 2025-01-15)
DEBUG Acquired lock for `C:\Users\Konsti\AppData\Roaming\uv\data\python`
DEBUG Found `cpython-3.13.1-windows-x86_64-none` for request `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Inspecting existing executable at `C:\Users\Konsti\.local\bin\python3.13.exe`
DEBUG Failed to inspect executable with error: Die Datei oder das Verzeichnis ist kein Analysepunkt. (os error 4390)
DEBUG Executable at `C:\Users\Konsti\.local\bin\python3.13.exe` is already for `cpython-3.13.1-windows-x86_64-none`
DEBUG Released lock at `C:\Users\Konsti\AppData\Roaming\uv\data\python\.lock`
```

---

_Label `windows` added by @konstin on 2025-01-15 13:26_

---

_Label `bug` added by @charliermarsh on 2025-01-15 16:37_

---

_Assigned to @zanieb by @zanieb on 2025-01-15 18:45_

---

_Closed by @zanieb on 2025-01-15 20:17_

---

_Closed by @zanieb on 2025-01-15 20:17_

---
