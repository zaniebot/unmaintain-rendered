```yaml
number: 5873
title: Use a larger runner for Windows test jobs
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/windows-larger-runner
created_at: 2024-08-07T16:20:53Z
updated_at: 2024-08-07T17:32:02Z
url: https://github.com/astral-sh/uv/pull/5873
synced_at: 2026-01-12T16:07:04Z
```

# Use a larger runner for Windows test jobs

---

_@zanieb_

From 8 to 16 cores, 32 to 64 GB ram. Testing on Windows first because it's the bottleneck.

Previously tested in #2515 to no effect, maybe better now that we have a development drive?


---

_Label `testing` added by @zanieb on 2024-08-07 16:20_

---

_Comment by @zanieb on 2024-08-07 16:39_

From 9m 21s to 6m 17s, nearly entirely in the `cargo test` step. The runner is twice as expensive, moving from $0.064 to $0.128 per minute. Since we've saving time, it'll be less than 2x expensive, but still more costly. Seems very worth it.

Note that 9m is one of the jobs on `main`, we've seen times as low as 7m.

---

_Marked ready for review by @zanieb on 2024-08-07 16:39_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-07 16:49_

---

_Merged by @zanieb on 2024-08-07 17:32_

---

_Closed by @zanieb on 2024-08-07 17:32_

---

_Branch deleted on 2024-08-07 17:32_

---
