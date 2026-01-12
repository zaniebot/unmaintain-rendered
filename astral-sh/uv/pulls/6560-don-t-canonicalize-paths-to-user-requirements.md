```yaml
number: 6560
title: "Don't canonicalize paths to user requirements"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/canogs
created_at: 2024-08-23T23:19:23Z
updated_at: 2024-08-24T02:02:15Z
url: https://github.com/astral-sh/uv/pull/6560
synced_at: 2026-01-12T16:07:25Z
```

# Don't canonicalize paths to user requirements

---

_@charliermarsh_

_No description provided._

---

_@zanieb reviewed on 2024-08-23 23:44_

---

_Review comment by @zanieb on `crates/uv-installer/src/plan.rs`:314 on 2024-08-23 23:44_

Should we have a helper for this like `user_display`? that was really helpful for usage.

---

_@zanieb approved on 2024-08-23 23:44_

---

_@charliermarsh reviewed on 2024-08-23 23:47_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:314 on 2024-08-23 23:47_

Yeah something like... `absolutize`?

---

_@charliermarsh reviewed on 2024-08-23 23:55_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:314 on 2024-08-23 23:55_

I'm going to do it in a different PR because I want to remove our `absolutize` dependency.

---

_Merged by @charliermarsh on 2024-08-24 02:02_

---

_Closed by @charliermarsh on 2024-08-24 02:02_

---

_Branch deleted on 2024-08-24 02:02_

---
