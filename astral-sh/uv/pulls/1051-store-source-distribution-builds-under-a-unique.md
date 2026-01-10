```yaml
number: 1051
title: Store source distribution builds under a unique manifest ID
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/id
created_at: 2024-01-22T22:33:47Z
updated_at: 2024-01-23T19:49:12Z
url: https://github.com/astral-sh/uv/pull/1051
synced_at: 2026-01-10T15:39:03Z
```

# Store source distribution builds under a unique manifest ID

---

_Pull request opened by @charliermarsh on 2024-01-22 22:33_

## Summary

This is a refactor of the source distribution cache that again aims to make the cache purely additive. Instead of deleting all built wheels when the cache gets invalidated (e.g., because the source distribution changed on PyPI or something), we now treat each invalidation as its own cache directory. The manifest inside of the source distribution directory now becomes a pointer to the "latest" version of the source distribution cache.

Here's a visual example:

![Screenshot 2024-01-22 at 5 35 41 PM](https://github.com/astral-sh/puffin/assets/1309177/ca103c83-e116-4956-b91c-8434fe62cffe)

With this change, we avoid deleting built distributions that might be relied on elsewhere and maintain our invariant that the cache is purely additive. The cost is that we now preserve stale wheels, but we should add a garbage collection mechanism to deal with that.


---

_Marked ready for review by @charliermarsh on 2024-01-22 22:36_

---

_Review requested from @konstin by @charliermarsh on 2024-01-22 22:36_

---

_@konstin approved on 2024-01-23 15:36_

---

_@konstin reviewed on 2024-01-23 15:36_

---

_Review comment by @konstin on `werkzeug-3.0.1.tar.gz`:1 on 2024-01-23 15:36_

This file needs to be deleted

---

_Merged by @charliermarsh on 2024-01-23 19:49_

---

_Closed by @charliermarsh on 2024-01-23 19:49_

---

_Branch deleted on 2024-01-23 19:49_

---
