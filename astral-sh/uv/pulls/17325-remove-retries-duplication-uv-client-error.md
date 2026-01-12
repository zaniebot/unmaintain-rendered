```yaml
number: 17325
title: Remove retries duplication uv_client error
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-retries-indirection
created_at: 2026-01-05T18:13:24Z
updated_at: 2026-01-06T09:20:04Z
url: https://github.com/astral-sh/uv/pull/17325
synced_at: 2026-01-12T16:12:43Z
```

# Remove retries duplication uv_client error

---

_@konstin_

Previously, we had a retry count both on the top level error type, and on an error variant, and had a conversion step in between. When reviewing #17274, I noticed we can simplify that.


---

_Review requested from @EliteTK by @konstin on 2026-01-05 18:13_

---

_Label `internal` added by @konstin on 2026-01-05 18:13_

---

_@EliteTK approved on 2026-01-05 20:08_

Nice!

---

_Merged by @konstin on 2026-01-06 09:20_

---

_Closed by @konstin on 2026-01-06 09:20_

---

_Branch deleted on 2026-01-06 09:20_

---
