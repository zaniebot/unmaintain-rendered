```yaml
number: 8049
title: Enable HTTP/2 in reqwest
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/http2
created_at: 2024-10-09T15:41:07Z
updated_at: 2024-10-09T16:58:26Z
url: https://github.com/astral-sh/uv/pull/8049
synced_at: 2026-01-10T12:54:01Z
```

# Enable HTTP/2 in reqwest

---

_Pull request opened by @konstin on 2024-10-09 15:41_

We should support HTTP/2, a default feature of reqwest, which as it seems was disabled as an oversight. With our many and parallel requests, this should give us even a smaller perf improvement, or at least decrease the load on the server/CDN by multiplexing and smaller headers (see e.g. https://www.cloudflare.com/learning/performance/http2-vs-http1.1/)

---

_Label `enhancement` added by @konstin on 2024-10-09 15:41_

---

_Review requested from @BurntSushi by @konstin on 2024-10-09 16:43_

---

_Review requested from @charliermarsh by @konstin on 2024-10-09 16:43_

---

_Review requested from @zanieb by @konstin on 2024-10-09 16:43_

---

_@charliermarsh approved on 2024-10-09 16:45_

---

_@fasterthanlime approved on 2024-10-09 16:55_

I suggested this in Discord and I approve of this change 🙏 

---

_@BurntSushi approved on 2024-10-09 16:57_

---

_@zanieb approved on 2024-10-09 16:57_

🎉 

---

_Merged by @konstin on 2024-10-09 16:58_

---

_Closed by @konstin on 2024-10-09 16:58_

---

_Branch deleted on 2024-10-09 16:58_

---
