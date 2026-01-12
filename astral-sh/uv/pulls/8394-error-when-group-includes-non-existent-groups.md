```yaml
number: 8394
title: Error when --group includes non-existent groups
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: tracking/735
head: charlie/error-groups
created_at: 2024-10-20T23:17:06Z
updated_at: 2024-10-22T02:08:33Z
url: https://github.com/astral-sh/uv/pull/8394
synced_at: 2026-01-12T16:08:18Z
```

# Error when --group includes non-existent groups

---

_@charliermarsh_

## Summary

Part of https://github.com/astral-sh/uv/pull/8272.


---

_@charliermarsh reviewed on 2024-10-20 23:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:618 on 2024-10-20 23:21_

I don't love using `metadata` here (rather than `root.dev_dependencies`) because this metadata is mostly just intended for lockfile validation / caching (we invalidate the lockfile when the metadata changes). But if we use `root.dev_dependencies`, we will _also_ fail if the user does `--group foo` when `foo` is empty. It seems wrong to fail when requesting a valid but empty group, so...

---

_@zanieb reviewed on 2024-10-21 16:40_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1258 on 2024-10-21 16:40_

Curious that we fail _after_ resolution?

---

_@zanieb approved on 2024-10-21 16:41_

---

_@charliermarsh reviewed on 2024-10-21 17:40_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1258 on 2024-10-21 17:40_

We validate the requested groups during the sync operation rather than the lock operation. I guess we could do both.

---

_@zanieb reviewed on 2024-10-21 17:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1258 on 2024-10-21 17:54_

Might be annoying to do a long resolution then fail because the group doesn't exist.

---

_Merged by @charliermarsh on 2024-10-22 02:08_

---

_Closed by @charliermarsh on 2024-10-22 02:08_

---

_Branch deleted on 2024-10-22 02:08_

---
