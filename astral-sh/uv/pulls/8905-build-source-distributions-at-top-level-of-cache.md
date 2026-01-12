```yaml
number: 8905
title: Build source distributions at top-level of cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - cache
assignees: []
merged: true
base: main
head: charlie/path
created_at: 2024-11-08T00:09:52Z
updated_at: 2024-11-08T00:39:05Z
url: https://github.com/astral-sh/uv/pull/8905
synced_at: 2026-01-12T16:08:33Z
```

# Build source distributions at top-level of cache

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/issues/8884. We build in a directory that's deep within the cache; to help with file name length limits, we should build at the top-level of the cache.


---

_Label `cache` added by @charliermarsh on 2024-11-08 00:09_

---

_Merged by @charliermarsh on 2024-11-08 00:20_

---

_Closed by @charliermarsh on 2024-11-08 00:20_

---

_Branch deleted on 2024-11-08 00:20_

---

_@zanieb reviewed on 2024-11-08 00:20_

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:1758 on 2024-11-08 00:20_

Sort of unclear that this returns a temporary directory and surprising that it's appropriate for building wheels in.

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:1758 on 2024-11-08 00:20_

@charliermarsh just pinging since you merged as I commented and it might get lost.

---

_@zanieb reviewed on 2024-11-08 00:20_

---

_@charliermarsh reviewed on 2024-11-08 00:23_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:1758 on 2024-11-08 00:23_

I think it's the intent of the bucket? I can look at making it clearer though.

---

_@zanieb reviewed on 2024-11-08 00:39_

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:1758 on 2024-11-08 00:39_

First it was unclear if the goal of the removed code was being upheld

>         // Prevent clashes from two uv processes building distributions in parallel.
>        let tmp_dir = tempdir_in(&wheel_dir)?;

Then I was confused by the documentation for the method

> /// Create an ephemeral Python environment in the cache.

I'm not super familiar with the cache methods though. Not a big deal either way.

---
