```yaml
number: 11412
title: Remove in-memory locks from distribution database
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/memory-locks
created_at: 2025-02-11T00:48:54Z
updated_at: 2025-02-21T04:57:33Z
url: https://github.com/astral-sh/uv/pull/11412
synced_at: 2026-01-10T11:10:37Z
```

# Remove in-memory locks from distribution database

---

_Pull request opened by @charliermarsh on 2025-02-11 00:48_

## Summary

I believe these are not necessary... They're currently used in two places:

1. When building wheels. But that's already wrapped in an in-flight map, which does the same thing.
2. When fetching source distribution metadata. But every route there uses it's own `flock` to coordinate across processes, so this seems redundant?


---

_Label `internal` added by @charliermarsh on 2025-02-11 00:48_

---

_Review requested from @ibraheemdev by @zanieb on 2025-02-11 14:42_

---

_Review request for @ibraheemdev removed by @zanieb on 2025-02-11 14:42_

---

_Review requested from @konstin by @zanieb on 2025-02-11 14:42_

---

_Review requested from @ibraheemdev by @zanieb on 2025-02-11 14:42_

---

_@konstin approved on 2025-02-12 09:54_

LGTM. It's hard to follow along if we covered all paths since we have no api-level connection between acquiring a lock and doing operations that need that lock acquired

---

_Merged by @charliermarsh on 2025-02-21 04:57_

---

_Closed by @charliermarsh on 2025-02-21 04:57_

---

_Branch deleted on 2025-02-21 04:57_

---
