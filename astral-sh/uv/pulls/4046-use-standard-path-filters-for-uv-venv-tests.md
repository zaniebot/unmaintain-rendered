```yaml
number: 4046
title: "Use standard path filters for `uv venv` tests"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/filter-standard
created_at: 2024-06-05T13:25:13Z
updated_at: 2024-06-05T15:25:11Z
url: https://github.com/astral-sh/uv/pull/4046
synced_at: 2026-01-10T13:54:02Z
```

# Use standard path filters for `uv venv` tests

---

_Pull request opened by @zanieb on 2024-06-05 13:25_

Over in #4045 the existing filters were insufficient. We should use the same strategy we use for our standard `TestContext` instead.

---

_Label `internal` added by @zanieb on 2024-06-05 13:25_

---

_Marked ready for review by @zanieb on 2024-06-05 13:26_

---

_@zanieb reviewed on 2024-06-05 14:01_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:34 on 2024-06-05 14:01_

Since we were passing the "absolute" but non-canonicalized path to CLI and we do not canonicalize paths to check if they're relative during user display, the user display differed on macOS.

---

_@zanieb reviewed on 2024-06-05 14:01_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:34 on 2024-06-05 14:01_

e.g. with debug output in `user_display`

```
path                           = "/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/.tmpnj31U6/.venv"
CWD.simplified()               = "/private/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/.tmpnj31U6"
CANONICAL_CWD.simplified()     = "/private/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/.tmpnj31U6"
path                           = "/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/.tmpnj31U6/.venv"
```

---

_Review requested from @konstin by @zanieb on 2024-06-05 14:14_

---

_@konstin approved on 2024-06-05 15:20_

---

_Merged by @zanieb on 2024-06-05 15:25_

---

_Closed by @zanieb on 2024-06-05 15:25_

---

_Branch deleted on 2024-06-05 15:25_

---
