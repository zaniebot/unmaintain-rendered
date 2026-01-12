```yaml
number: 10213
title: Update failure on Win10 64-bit
type: issue
state: closed
author: farzbood
labels:
  - bug
assignees: []
created_at: 2024-12-28T05:58:02Z
updated_at: 2024-12-28T14:38:09Z
url: https://github.com/astral-sh/uv/issues/10213
synced_at: 2026-01-12T16:00:08Z
```

# Update failure on Win10 64-bit

---

_@farzbood_

Hi everyone,
Attempting update with (uv self update) command to the latest version (0.5.13), failed with following error.

PS C:\Windows\system32> uv self update
info: Checking for updates...
error: error decoding response body
  Caused by: there are extra bytes after body has been decompressed

O.S: windows 10 x86-64

---

_Comment by @FishAlchemist on 2024-12-28 09:34_

uv version is 0.5.12, right? The self-update for this version is broken. Please refer to the comment(https://github.com/astral-sh/uv/issues/10196#issuecomment-2563857968) for the fix.

---

_Comment by @charliermarsh on 2024-12-28 14:38_

👍 There was an issue in v0.5.12. To upgrade, I recommend re-running the installer:

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Subsequent self-updates should work as expected.

---

_Closed by @charliermarsh on 2024-12-28 14:38_

---

_Label `bug` added by @charliermarsh on 2024-12-28 14:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-28 14:38_

---
