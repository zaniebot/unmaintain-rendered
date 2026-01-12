```yaml
number: 529
title: Invalidate interpreter marker cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2023-12-03T22:22:34Z
updated_at: 2023-12-03T22:44:44Z
url: https://github.com/astral-sh/uv/pull/529
synced_at: 2026-01-12T16:04:00Z
```

# Invalidate interpreter marker cache

---

_@charliermarsh_

In a refactor, we lost the cache invalidation behavior for interpreter markers, leading to stale interpreter errors for me when creating environments with different Python versions. Specifically, the modification timestamp used to be part of the _cache key_ when we used `cacache`. Now it's not -- but it's stored within the cache. So we need to validate the key after-the-fact.


---

_Label `bug` added by @charliermarsh on 2023-12-03 22:22_

---

_Merged by @charliermarsh on 2023-12-03 22:44_

---

_Closed by @charliermarsh on 2023-12-03 22:44_

---

_Branch deleted on 2023-12-03 22:44_

---
