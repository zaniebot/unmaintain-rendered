```yaml
number: 10837
title: Remove extra collects when flattening requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/alloc
created_at: 2025-01-22T02:03:32Z
updated_at: 2025-01-22T15:14:09Z
url: https://github.com/astral-sh/uv/pull/10837
synced_at: 2026-01-10T11:45:14Z
```

# Remove extra collects when flattening requirements

---

_Pull request opened by @charliermarsh on 2025-01-22 02:03_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2025-01-22 02:03_

---

_Review requested from @konstin by @charliermarsh on 2025-01-22 02:23_

---

_@charliermarsh reviewed on 2025-01-22 02:24_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1801 on 2025-01-22 02:24_

We no longer need to collect into a `Vec` in the fast paths.

---

_@konstin approved on 2025-01-22 12:31_

---

_Merged by @charliermarsh on 2025-01-22 15:14_

---

_Closed by @charliermarsh on 2025-01-22 15:14_

---

_Branch deleted on 2025-01-22 15:14_

---
