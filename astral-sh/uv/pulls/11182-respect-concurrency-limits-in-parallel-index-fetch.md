```yaml
number: 11182
title: Respect concurrency limits in parallel index fetch
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/respect-concurrency-limits
created_at: 2025-02-03T13:50:06Z
updated_at: 2025-02-03T15:41:20Z
url: https://github.com/astral-sh/uv/pull/11182
synced_at: 2026-01-12T16:09:43Z
```

# Respect concurrency limits in parallel index fetch

---

_@konstin_

With the parallel simple index fetching, we would only acquire one download concurrency token, meaning that we could in the worst case make times the number of indexes more requests than the user requested limit. We fix this by passing the semaphore down to the simple API method.

---

_Label `bug` added by @konstin on 2025-02-03 13:50_

---

_Review requested from @charliermarsh by @konstin on 2025-02-03 13:50_

---

_@charliermarsh approved on 2025-02-03 15:35_

---

_Merged by @konstin on 2025-02-03 15:41_

---

_Closed by @konstin on 2025-02-03 15:41_

---

_Branch deleted on 2025-02-03 15:41_

---
