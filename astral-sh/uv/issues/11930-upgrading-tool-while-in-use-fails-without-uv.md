---
number: 11930
title: Upgrading tool while in use fails without uv recognizing the failed update on subsequent upgrade attempts
type: issue
state: open
author: nbaju1
labels:
  - bug
assignees: []
created_at: 2025-03-03T18:14:47Z
updated_at: 2025-03-03T18:53:43Z
url: https://github.com/astral-sh/uv/issues/11930
synced_at: 2026-01-10T01:25:12Z
---

# Upgrading tool while in use fails without uv recognizing the failed update on subsequent upgrade attempts

---

_Issue opened by @nbaju1 on 2025-03-03 18:14_

### Summary

Steps to reproduce:
1. Run `uv tool install ruff@0.9.1`
2. Modify the uv-receipt.toml file for ruff incrementing the specifier to 0.9.2.
3. Open e.g. Cursor (or some IDE/program that use the ruff binary in the bin folder).
4. Run `uv tool upgrade ruff`.
5. Run `ruff --version`
6. Repeat step 4.

Output of step 4.
```
error: Failed to upgrade ruff
  Caused by: Failed to install entrypoint
  Caused by: failed to copy file from C:\Users\<user>\AppData\Roaming\uv\tools\ruff\Scripts\ruff.exe to C:\Users\<user>\.local\bin\ruff.exe: The process cannot access the file because it is being used by another process. (os error 32)
```

Output of step 5:
```
ruff 0.9.1
```

Output of step 6 (specifier still says 0.9.2 in uv-receipt.toml):
```
Nothing to upgrade
```

### Platform

Windows 11 x64

### Version

0.6.3

### Python version

Python 3.12.7

---

_Label `bug` added by @nbaju1 on 2025-03-03 18:14_

---

_Renamed from "Upgrading tools while in use fails without uv recognizing the failed update" to "Upgrading tool while in use fails without uv recognizing the failed update on subsequent upgrade attempts" by @nbaju1 on 2025-03-03 18:15_

---

_Referenced in [astral-sh/uv#14520](../../astral-sh/uv/issues/14520.md) on 2025-07-09 14:48_

---
