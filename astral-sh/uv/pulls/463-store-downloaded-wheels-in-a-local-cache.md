```yaml
number: 463
title: Store downloaded wheels in a local cache
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2023-11-19T23:27:36Z
updated_at: 2023-11-20T11:32:23Z
url: https://github.com/astral-sh/uv/pull/463
synced_at: 2026-01-10T15:50:29Z
```

# Store downloaded wheels in a local cache

---

_Pull request opened by @charliermarsh on 2023-11-19 23:27_

This PR modifies the `Fetcher` to cache remote wheels that we _already_ store to-disk. We might read these again in the future, so we might as well store them in the cache for consistency (rather than using a temporary directory).

---

_Marked ready for review by @charliermarsh on 2023-11-19 23:27_

---

_Review requested from @konstin by @charliermarsh on 2023-11-19 23:27_

---

_@konstin approved on 2023-11-20 08:56_

---

_Merged by @charliermarsh on 2023-11-20 11:32_

---

_Closed by @charliermarsh on 2023-11-20 11:32_

---

_Branch deleted on 2023-11-20 11:32_

---
