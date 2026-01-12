```yaml
number: 8349
title: Log unfiltered snapshots on failure
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/snap-filt
created_at: 2024-10-18T23:20:23Z
updated_at: 2024-10-21T08:30:09Z
url: https://github.com/astral-sh/uv/pull/8349
synced_at: 2026-01-12T16:08:16Z
```

# Log unfiltered snapshots on failure

---

_@zanieb_

Cherry-picked from https://github.com/astral-sh/uv/pull/8347

Seems generally really helpful to see the unfiltered snapshot when a test fails. Especially when debugging filters on Windows.

---

_Label `internal` added by @zanieb on 2024-10-18 23:20_

---

_Comment by @zanieb on 2024-10-18 23:43_

For example, a random test I modified to fail

<img width="1794" alt="Screenshot 2024-10-18 at 6 43 06 PM" src="https://github.com/user-attachments/assets/1decadb2-af4c-462c-9e66-095c7a3b1fce">


---

_Marked ready for review by @zanieb on 2024-10-18 23:46_

---

_Label `testing` added by @zanieb on 2024-10-18 23:49_

---

_@zanieb reviewed on 2024-10-18 23:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1151 on 2024-10-18 23:50_

Note we write this unconditionally, it's just displayed on failure.

Initially I wrote the whole snapshot unfiltered, but it seems like this is all that we want here.

---

_@charliermarsh approved on 2024-10-20 16:12_

---

_Merged by @zanieb on 2024-10-20 18:37_

---

_Closed by @zanieb on 2024-10-20 18:37_

---

_Branch deleted on 2024-10-20 18:37_

---

_Comment by @konstin on 2024-10-21 08:30_

This is very helpful!

---
