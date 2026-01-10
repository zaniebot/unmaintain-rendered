```yaml
number: 957
title: Mark path-based cache entries as stale during install plan
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/timestamp
created_at: 2024-01-18T04:08:33Z
updated_at: 2024-01-18T19:13:31Z
url: https://github.com/astral-sh/uv/pull/957
synced_at: 2026-01-10T15:39:03Z
```

# Mark path-based cache entries as stale during install plan

---

_Pull request opened by @charliermarsh on 2024-01-18 04:08_

## Summary

This is a small correctness improvement that ensures that we avoid using stale cache entries for local dependencies in the install plan. We already have some logic like this in the source distribution builder, but it didn't apply in the install plan, and so we'd end up using stale wheels.

Specifically, now, if you create a new local wheel, and run `pip sync`, we'll mark the cache entries as stale and make sure we unzip it and install it. (If the wheel is _already_ installed, we won't reinstall it though, which will be a separate change. This is just about reading from the cache, not the environment.)

---

_Review requested from @zanieb by @charliermarsh on 2024-01-18 04:10_

---

_Review requested from @konstin by @charliermarsh on 2024-01-18 04:10_

---

_Label `bug` added by @charliermarsh on 2024-01-18 04:10_

---

_@charliermarsh reviewed on 2024-01-18 04:10_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/index/built_wheel_index.rs`:44 on 2024-01-18 04:10_

We get to remove a TODO.

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_sync.rs`:1152 on 2024-01-18 11:59_

We can set the mtime to `SystemTime::now()` instead https://doc.rust-lang.org/std/fs/struct.File.html#method.set_times

---

_@konstin approved on 2024-01-18 12:01_

---

_Merged by @charliermarsh on 2024-01-18 19:13_

---

_Closed by @charliermarsh on 2024-01-18 19:13_

---

_Branch deleted on 2024-01-18 19:13_

---
