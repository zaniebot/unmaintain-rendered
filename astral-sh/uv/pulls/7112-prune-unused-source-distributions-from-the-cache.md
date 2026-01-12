```yaml
number: 7112
title: Prune unused source distributions from the cache
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
created_at: 2024-09-06T01:27:47Z
updated_at: 2024-09-06T01:40:53Z
url: https://github.com/astral-sh/uv/pull/7112
synced_at: 2026-01-12T16:07:42Z
```

# Prune unused source distributions from the cache

---

_@charliermarsh_

## Summary

This has bothered me for a while and should be fairly impactful for users. It requires a weird implementation, since the distribution-building crate depends on the cache, and so the prune operation can't live in the cache, since it needs to access internals of the distribution-building crate.

Closes https://github.com/astral-sh/uv/issues/7096.


---

_Label `enhancement` added by @charliermarsh on 2024-09-06 01:27_

---

_Label `cache` added by @charliermarsh on 2024-09-06 01:27_

---

_Comment by @charliermarsh on 2024-09-06 01:32_

The actual thing preventing this from living in `uv-cache` is that it needs `DataWithCachePolicy` which is part of `uv-client`.

---

_Merged by @charliermarsh on 2024-09-06 01:40_

---

_Closed by @charliermarsh on 2024-09-06 01:40_

---

_Branch deleted on 2024-09-06 01:40_

---
