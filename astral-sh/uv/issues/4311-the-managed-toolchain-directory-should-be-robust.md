```yaml
number: 4311
title: The managed toolchain directory should be robust to concurrent access
type: issue
state: closed
author: zanieb
labels:
  - preview
assignees: []
created_at: 2024-06-13T17:12:29Z
updated_at: 2024-07-09T10:41:32Z
url: https://github.com/astral-sh/uv/issues/4311
synced_at: 2026-01-12T15:58:49Z
```

# The managed toolchain directory should be robust to concurrent access

---

_@zanieb_

e.g. fetches should be safe to run concurrently and _reading_ a toolchain should not occur mid-fetch.

I believe we won't already read a toolchain mid-fetch because we download into a temporary directory then rename.

Do we need to lock the toolchain directory when fetching the same version concurrently? Or do we just let the later one overwrite the first?

---

_Label `preview` added by @zanieb on 2024-06-13 17:12_

---

_Comment by @konstin on 2024-07-09 10:41_

Fixed by https://github.com/astral-sh/uv/pull/4733

---

_Closed by @konstin on 2024-07-09 10:41_

---
