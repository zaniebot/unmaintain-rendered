```yaml
number: 2985
title: Store IDs rather than paths in the cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/id
created_at: 2024-04-11T00:45:31Z
updated_at: 2024-04-11T01:07:52Z
url: https://github.com/astral-sh/uv/pull/2985
synced_at: 2026-01-12T16:05:21Z
```

# Store IDs rather than paths in the cache

---

_@charliermarsh_

## Summary

Similar to `Revision`, we now store IDs in the `Archive` entires rather than absolute paths. This makes the cache robust to moves, etc.

Closes https://github.com/astral-sh/uv/issues/2908.


---

_Label `internal` added by @charliermarsh on 2024-04-11 00:45_

---

_Marked ready for review by @charliermarsh on 2024-04-11 00:45_

---

_Merged by @charliermarsh on 2024-04-11 01:07_

---

_Closed by @charliermarsh on 2024-04-11 01:07_

---

_Branch deleted on 2024-04-11 01:07_

---
