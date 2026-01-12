```yaml
number: 6779
title: Avoid using debug representation for git source urls
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/fix-git-source-log
created_at: 2024-08-28T21:52:47Z
updated_at: 2024-08-28T22:02:25Z
url: https://github.com/astral-sh/uv/pull/6779
synced_at: 2026-01-12T16:07:32Z
```

# Avoid using debug representation for git source urls

---

_@zanieb_

e.g.

> DEBUG Using existing Git source `https://github.com/StarfishStorage/python-swiftclient.git`

instead of

> DEBUG Using existing git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/StarfishStorage/gunicorn.git", query: None, fragment: None }`

---

_Label `tracing` added by @zanieb on 2024-08-28 21:52_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-28 21:59_

---

_@charliermarsh approved on 2024-08-28 22:00_

Thanks!

---

_Merged by @zanieb on 2024-08-28 22:02_

---

_Closed by @zanieb on 2024-08-28 22:02_

---

_Branch deleted on 2024-08-28 22:02_

---
