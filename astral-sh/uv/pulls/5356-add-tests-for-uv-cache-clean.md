```yaml
number: 5356
title: "Add tests for `uv cache clean`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/cache-clean
created_at: 2024-07-23T17:44:08Z
updated_at: 2024-07-23T18:03:03Z
url: https://github.com/astral-sh/uv/pull/5356
synced_at: 2026-01-12T16:06:46Z
```

# Add tests for `uv cache clean`

---

_@charliermarsh_

_No description provided._

---

_Label `testing` added by @charliermarsh on 2024-07-23 17:44_

---

_Marked ready for review by @charliermarsh on 2024-07-23 17:44_

---

_@zanieb reviewed on 2024-07-23 17:45_

---

_Review comment by @zanieb on `crates/uv/tests/cache_clean.rs`:21 on 2024-07-23 17:45_

Technically this is bad form, can you add it to the `TestContext` to match the other commands?

---

_Review comment by @charliermarsh on `crates/uv/tests/cache_clean.rs`:21 on 2024-07-23 17:52_

Yup.

---

_@charliermarsh reviewed on 2024-07-23 17:52_

---

_@charliermarsh reviewed on 2024-07-23 17:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/cache_clean.rs`:21 on 2024-07-23 17:54_

(I cribbed this from `cache_prune.rs` so changing that too.)

---

_@zanieb reviewed on 2024-07-23 17:58_

---

_Review comment by @zanieb on `crates/uv/tests/cache_clean.rs`:21 on 2024-07-23 17:58_

Thanks! 

---

_@zanieb approved on 2024-07-23 17:59_

---

_Merged by @charliermarsh on 2024-07-23 18:03_

---

_Closed by @charliermarsh on 2024-07-23 18:03_

---

_Branch deleted on 2024-07-23 18:03_

---
