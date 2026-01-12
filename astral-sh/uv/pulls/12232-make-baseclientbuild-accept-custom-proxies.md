```yaml
number: 12232
title: make BaseClientBuild accept custom proxies
type: pull_request
state: merged
author: gzm55
labels:
  - rustlib
assignees: []
merged: true
base: main
head: work/proxy
created_at: 2025-03-17T09:14:21Z
updated_at: 2025-03-17T19:18:56Z
url: https://github.com/astral-sh/uv/pull/12232
synced_at: 2026-01-12T16:10:11Z
```

# make BaseClientBuild accept custom proxies

---

_@gzm55_

close #12230 

implement `BaseClientBuilder::proxy(self, Proxy)` and apply it to the underlying reqwest::Client.

---

_@konstin approved on 2025-03-17 10:12_

---

_Review requested from @zanieb by @konstin on 2025-03-17 10:12_

---

_Label `rustlib` added by @zanieb on 2025-03-17 18:46_

---

_Merged by @zanieb on 2025-03-17 19:18_

---

_Closed by @zanieb on 2025-03-17 19:18_

---
