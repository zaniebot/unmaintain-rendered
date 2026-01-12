```yaml
number: 2764
title: Use canonical URL to key redirect map
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/canonical
created_at: 2024-04-01T21:46:58Z
updated_at: 2024-04-01T21:57:14Z
url: https://github.com/astral-sh/uv/pull/2764
synced_at: 2026-01-12T16:05:12Z
```

# Use canonical URL to key redirect map

---

_@charliermarsh_

## Summary

This fixes a potential bug that revealed itself in https://github.com/astral-sh/uv/pull/2761. We don't run into this now because we always use "allowed URLs", stores the "last" compatible URL in the map. But the use of the "raw" URL (rather than the "canonical" URL) means that other writers have to follow that same assumption and iterate over dependencies in-order.


---

_Label `bug` added by @charliermarsh on 2024-04-01 21:47_

---

_Marked ready for review by @charliermarsh on 2024-04-01 21:47_

---

_Merged by @charliermarsh on 2024-04-01 21:57_

---

_Closed by @charliermarsh on 2024-04-01 21:57_

---

_Branch deleted on 2024-04-01 21:57_

---
