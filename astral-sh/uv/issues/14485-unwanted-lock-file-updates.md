```yaml
number: 14485
title: Unwanted lock file updates
type: issue
state: closed
author: melMass
labels:
  - bug
assignees: []
created_at: 2025-07-07T13:53:57Z
updated_at: 2025-07-08T01:10:36Z
url: https://github.com/astral-sh/uv/issues/14485
synced_at: 2026-01-12T16:01:49Z
```

# Unwanted lock file updates

---

_@melMass_

### Summary

In a workspace where I use torch nightly, whenever I interact with uv (like `uv add`) it updates torch and the lock file to latest nightly... I can't find how to avoid the lock file from updating it as I want to manually issue `-U`

Full repro here: https://github.com/melMass/reproduction/tree/python/uv-add-updates-lock


https://github.com/user-attachments/assets/75160931-5fd7-4c6b-a957-abf7e1d3b77f

### Platform

Windows 11 x64

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

### Python version

3.11.11

---

_Label `bug` added by @melMass on 2025-07-07 13:53_

---

_Assigned to @zanieb by @zanieb on 2025-07-07 13:54_

---

_Comment by @zanieb on 2025-07-07 20:49_

I can reproduce this, thanks!

---

_Comment by @zanieb on 2025-07-07 21:39_

Is this resolved if you set

```
[tool.uv]
prerelease = "allow"
```

I believe the problem is that we're filtering out the existing preference for the pre-release `dev` version.

(It won't work immediately, as adding this setting invalidates the lockfile, but it should work after that)

---

_Comment by @melMass on 2025-07-07 22:00_

I added it! Let's see if it happens soon, it's a project I work on daily (comfyui) and torch nightlies are very frequent. I'll post updates here.

---

_Closed by @zanieb on 2025-07-08 01:10_

---
