```yaml
number: 5874
title: Use a larger runner for macOS test jobs
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/macos-larger-runner
created_at: 2024-08-07T16:47:35Z
updated_at: 2024-08-07T17:32:10Z
url: https://github.com/astral-sh/uv/pull/5874
synced_at: 2026-01-12T16:07:04Z
```

# Use a larger runner for macOS test jobs

---

_@zanieb_

From 3 to 6 (+8 GPU) cores, 7 to 14 GB ram.

Related:
- https://github.com/astral-sh/uv/pull/5873


---

_Label `testing` added by @zanieb on 2024-08-07 16:47_

---

_Comment by @zanieb on 2024-08-07 16:54_

From 6m 8s to 4m 16s, as with #5873, entirely in the `cargo test` step. We should check the cost, but it seems worth it regardless.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-07 16:54_

---

_Marked ready for review by @zanieb on 2024-08-07 16:54_

---

_Merged by @zanieb on 2024-08-07 17:32_

---

_Closed by @zanieb on 2024-08-07 17:32_

---

_Branch deleted on 2024-08-07 17:32_

---
