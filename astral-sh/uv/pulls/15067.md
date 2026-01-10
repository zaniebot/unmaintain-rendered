```yaml
number: 15067
title: Improve HTTP response caching log messages
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/cache-trace
created_at: 2025-08-04T21:51:36Z
updated_at: 2025-08-05T19:34:14Z
url: https://github.com/astral-sh/uv/pull/15067
synced_at: 2026-01-10T06:44:33Z
```

# Improve HTTP response caching log messages

---

_Pull request opened by @zanieb on 2025-08-04 21:51_

"Cached request ... is not storable" doesn't make sense from a user perspective, it's leaking our internal `CachedClient` abstraction. I think it makes more sense to talk about this as "Response from ... is not storable"

---

_Label `tracing` added by @zanieb on 2025-08-04 21:51_

---

_Renamed from "Improve HTTP response caching logs" to "Improve HTTP response caching log messages" by @zanieb on 2025-08-04 23:29_

---

_@jtfmumm approved on 2025-08-05 19:28_

---

_Merged by @zanieb on 2025-08-05 19:34_

---

_Closed by @zanieb on 2025-08-05 19:34_

---

_Branch deleted on 2025-08-05 19:34_

---
