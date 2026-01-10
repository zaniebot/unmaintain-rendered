```yaml
number: 5441
title: Update to packse 0.3.31
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/packse-0-3-31
created_at: 2024-07-25T13:42:37Z
updated_at: 2024-07-26T15:39:31Z
url: https://github.com/astral-sh/uv/pull/5441
synced_at: 2026-01-10T13:37:23Z
```

# Update to packse 0.3.31

---

_Pull request opened by @konstin on 2024-07-25 13:42_

Update packse to 0.3.31, adding the instability scenarios.

---

_@konstin reviewed on 2024-07-25 13:44_

---

_Review comment by @konstin on `crates/uv/tests/lock_scenarios.rs`:2353 on 2024-07-25 13:44_

@zanieb Should it be collapsing the markdown?

---

_@zanieb reviewed on 2024-07-25 14:34_

---

_Review comment by @zanieb on `crates/uv/tests/lock_scenarios.rs`:2353 on 2024-07-25 14:34_

Seems less than ideal. It seems fine to leave it on multiple lines? I'm not sure why this is happening.

---

_@konstin reviewed on 2024-07-26 15:25_

---

_Review comment by @konstin on `crates/uv/tests/lock_scenarios.rs`:2353 on 2024-07-26 15:25_

https://github.com/astral-sh/packse/pull/205

---

_Marked ready for review by @konstin on 2024-07-26 15:31_

---

_Comment by @konstin on 2024-07-26 15:38_

I'm going ahead with this to unblock using these tests for #5180. We can fix the comments in a later update.

---

_Merged by @konstin on 2024-07-26 15:39_

---

_Closed by @konstin on 2024-07-26 15:39_

---

_Branch deleted on 2024-07-26 15:39_

---
