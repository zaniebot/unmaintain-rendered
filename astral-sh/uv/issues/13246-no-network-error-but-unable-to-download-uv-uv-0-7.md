---
number: 13246
title: No network error but unable to download uv (uv 0.7.2 x86_64-unknown-linux-gnu
type: issue
state: closed
author: reginaldc
labels:
  - bug
assignees: []
created_at: 2025-05-01T05:47:04Z
updated_at: 2025-05-01T18:15:36Z
url: https://github.com/astral-sh/uv/issues/13246
synced_at: 2026-01-10T01:25:30Z
---

# No network error but unable to download uv (uv 0.7.2 x86_64-unknown-linux-gnu

---

_Issue opened by @reginaldc on 2025-05-01 05:47_

### Summary

curl: (23) client returned ERROR on write of 16375 bytes
failed to download https://github.com/astral-sh/uv/releases/download/0.7.2/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt


### Platform

ubuntu 24.4

### Version

uv 0.7.2 x86_64-unknown-linux-gnu

### Python version

_No response_

---

_Label `bug` added by @reginaldc on 2025-05-01 05:47_

---

_Comment by @zanieb on 2025-05-01 18:15_

I can confirm the release is fine — it sounds like a spurious network error.

Thanks!

---

_Closed by @zanieb on 2025-05-01 18:15_

---
