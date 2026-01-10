```yaml
number: 6790
title: Avoid deadlocks when multiple uv processes lock resources
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/lock-file-async
created_at: 2024-08-29T04:20:05Z
updated_at: 2024-08-29T16:16:17Z
url: https://github.com/astral-sh/uv/pull/6790
synced_at: 2026-01-10T12:53:34Z
```

# Avoid deadlocks when multiple uv processes lock resources

---

_Pull request opened by @zanieb on 2024-08-29 04:20_

This is achieved by updating the `LockedFile::acquire` API to be async — as in some cases we were attempting to acquire the lock synchronously, i.e., without yielding, which blocked the runtime. 

Closes https://github.com/astral-sh/uv/issues/6691 — I tested with the reproduction there and a local release build and no longer reproduce the deadlock with these changes.

Some additional context in the [internal Discord thread](https://discord.com/channels/1039017663004942429/1278430431204741270/1278478941262188595)

---

_Label `bug` added by @zanieb on 2024-08-29 04:20_

---

_@zanieb reviewed on 2024-08-29 04:21_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:355 on 2024-08-29 04:21_

We could remove this entirely too.

---

_@zanieb reviewed on 2024-08-29 04:22_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:364 on 2024-08-29 04:22_

I need to do this for `spawn_blocking`. Are there better ways?

---

_Review requested from @charliermarsh by @zanieb on 2024-08-29 04:23_

---

_Review requested from @konstin by @zanieb on 2024-08-29 04:23_

---

_Marked ready for review by @zanieb on 2024-08-29 04:23_

---

_@konstin approved on 2024-08-29 09:20_

---

_@charliermarsh reviewed on 2024-08-29 12:57_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:364 on 2024-08-29 12:57_

I think it's ok, I believe it needs to have a static lifetime (or be owned) for this...

---

_@charliermarsh approved on 2024-08-29 12:57_

---

_Merged by @zanieb on 2024-08-29 16:16_

---

_Closed by @zanieb on 2024-08-29 16:16_

---

_Branch deleted on 2024-08-29 16:16_

---
