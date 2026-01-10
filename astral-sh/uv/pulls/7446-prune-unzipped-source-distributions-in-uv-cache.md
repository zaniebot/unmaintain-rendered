```yaml
number: 7446
title: "Prune unzipped source distributions in `uv cache prune --ci`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cache
assignees: []
merged: true
base: main
head: charlie/prune
created_at: 2024-09-16T23:01:48Z
updated_at: 2024-09-16T23:18:22Z
url: https://github.com/astral-sh/uv/pull/7446
synced_at: 2026-01-10T12:53:47Z
```

# Prune unzipped source distributions in `uv cache prune --ci`

---

_Pull request opened by @charliermarsh on 2024-09-16 23:01_

## Summary

It's very unlikely that retaining these is beneficial, since you tend to partition the cache by platform anyway.

Closes https://github.com/astral-sh/uv/issues/7394.


---

_Review requested from @konstin by @charliermarsh on 2024-09-16 23:01_

---

_Label `enhancement` added by @charliermarsh on 2024-09-16 23:01_

---

_Label `cache` added by @charliermarsh on 2024-09-16 23:01_

---

_Merged by @charliermarsh on 2024-09-16 23:18_

---

_Closed by @charliermarsh on 2024-09-16 23:18_

---

_Branch deleted on 2024-09-16 23:18_

---
