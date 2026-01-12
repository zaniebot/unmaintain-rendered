```yaml
number: 11622
title: "Revert: Include environment variables in interpreter info caching"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/revert-python-env-var-cache-key
created_at: 2025-02-19T15:21:18Z
updated_at: 2025-02-19T16:10:24Z
url: https://github.com/astral-sh/uv/pull/11622
synced_at: 2026-01-12T16:09:55Z
```

# Revert: Include environment variables in interpreter info caching

---

_@konstin_

Revert #11601 for now

We run Python interpreter discovery with `-I` (#2500) which means these environments variables are ignored when determining `sys.path`. Unless we decide to remove the `-I` flag from the `sys.path` query, we shouldn't release these changes to interpreter discovery caching.

---

_@zanieb approved on 2025-02-19 15:56_

---

_Merged by @zanieb on 2025-02-19 16:10_

---

_Closed by @zanieb on 2025-02-19 16:10_

---

_Branch deleted on 2025-02-19 16:10_

---
