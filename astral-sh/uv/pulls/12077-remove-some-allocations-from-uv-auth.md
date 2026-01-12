```yaml
number: 12077
title: "Remove some allocations from `uv-auth`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - no-build
assignees: []
merged: true
base: main
head: charlie/au
created_at: 2025-03-09T17:57:46Z
updated_at: 2025-03-09T18:28:35Z
url: https://github.com/astral-sh/uv/pull/12077
synced_at: 2026-01-12T16:10:07Z
```

# Remove some allocations from `uv-auth`

---

_@charliermarsh_

## Summary

Use `SmallString`, and no need to allocate a `String` to fetch from the URLs cache.


---

_Label `performance` added by @charliermarsh on 2025-03-09 17:57_

---

_Label `no-build` added by @charliermarsh on 2025-03-09 18:01_

---

_Merged by @charliermarsh on 2025-03-09 18:28_

---

_Closed by @charliermarsh on 2025-03-09 18:28_

---

_Branch deleted on 2025-03-09 18:28_

---
