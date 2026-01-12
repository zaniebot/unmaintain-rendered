```yaml
number: 8350
title: "Set `UV_LINK_MODE=copy` for Windows test runs"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/win-copy-test
created_at: 2024-10-18T23:35:55Z
updated_at: 2024-10-20T18:37:43Z
url: https://github.com/astral-sh/uv/pull/8350
synced_at: 2026-01-12T16:08:17Z
```

# Set `UV_LINK_MODE=copy` for Windows test runs

---

_@zanieb_

Cherry-picked from #8347 

Might fix https://github.com/astral-sh/uv/issues/6940 — I'm not seeing a failure over there after this change. I think there may be some problem with concurrent reads of junctioned files on the DevDrive? It's really hard to say.

We might lose some important test coverage with this change. I'm not sure what to do about that either.

---

_Label `internal` added by @zanieb on 2024-10-18 23:35_

---

_Comment by @zanieb on 2024-10-18 23:44_

So annoying... this breaks a bunch of tests because it's in the snapshots.

---

_Marked ready for review by @zanieb on 2024-10-19 04:25_

---

_@charliermarsh approved on 2024-10-20 16:11_

---

_Merged by @zanieb on 2024-10-20 18:37_

---

_Closed by @zanieb on 2024-10-20 18:37_

---

_Branch deleted on 2024-10-20 18:37_

---
