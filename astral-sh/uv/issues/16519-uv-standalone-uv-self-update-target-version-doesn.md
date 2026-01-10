---
number: 16519
title: "uv standalone: `uv self update [TARGET_VERSION]` doesn't work in a mirrored/offline/non-internet environment"
type: issue
state: open
author: zamanh
labels:
  - bug
assignees: []
created_at: 2025-10-30T16:25:29Z
updated_at: 2025-10-30T17:09:16Z
url: https://github.com/astral-sh/uv/issues/16519
synced_at: 2026-01-10T01:26:07Z
---

# uv standalone: `uv self update [TARGET_VERSION]` doesn't work in a mirrored/offline/non-internet environment

---

_Issue opened by @zamanh on 2025-10-30 16:25_

### Summary

After following guidance to make uv install in an offline environment stated here: https://github.com/astral-sh/uv/issues/9134#issuecomment-2599266736

When setting `UV_DOWNLOAD_URL` (to point at the intended version of uv we want to upgrade to) and `UV_INSTALLER_GHE_BASE_URL` (to be our internal Artifactory instance): `uv self update 0.9.6` returns:

```powershell
PS C:\Users\Administrator> uv self update 0.9.6
info: Checking for updates...
error: The version 0.9.6 was not found for the app uv in workspace uv
```

### Platform

Windows/Linux x64

### Version

0.9.5

### Python version

n/a

---

_Label `bug` added by @zamanh on 2025-10-30 16:25_

---

_Comment by @zanieb on 2025-10-30 16:50_

When did it stop working? e.g., did you upgrade uv then encounter this?

---

_Comment by @zamanh on 2025-10-30 17:06_

Ah my issue title is a bit misleading, it's the first time I have tried to do this and have discovered this to be the case. Will update title. It works as usual up until the point of setting env vars to have it work though a mirror.

---

_Renamed from "uv standalone: `uv self update [TARGET_VERSION]` no longer functions in a mirrored/offline/non-internet environment" to "uv standalone: `uv self update [TARGET_VERSION]` doesn't work in a mirrored/offline/non-internet environment" by @zamanh on 2025-10-30 17:06_

---

_Comment by @zanieb on 2025-10-30 17:09_

Oh, I see. Thanks! This is implemented in a dependency, but we can look into it.

cc @Gankra 

---
