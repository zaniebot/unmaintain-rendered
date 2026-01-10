```yaml
number: 304
title: Use locks to prevent concurrent accesses to the same Git repo
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lock
created_at: 2023-11-03T00:12:49Z
updated_at: 2023-11-03T16:33:15Z
url: https://github.com/astral-sh/uv/pull/304
synced_at: 2026-01-10T15:50:28Z
```

# Use locks to prevent concurrent accesses to the same Git repo

---

_Pull request opened by @charliermarsh on 2023-11-03 00:12_

Ensures that if we need to access the same Git repo twice in a resolution, we only have one handler to that repo at a time. (Otherwise, `git2` panics.)

---

_Review requested from @konstin by @charliermarsh on 2023-11-03 13:41_

---

_@charliermarsh reviewed on 2023-11-03 13:42_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:882 on 2023-11-03 13:42_

@konstin - We may need these to be on the global builder context in some way.

---

_@zanieb reviewed on 2023-11-03 14:44_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_git_concurrent_access.snap`:22 on 2023-11-03 14:44_

Huh how is this okay?

---

_@zanieb approved on 2023-11-03 14:44_

---

_@charliermarsh reviewed on 2023-11-03 14:49_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__compile_git_concurrent_access.snap`:22 on 2023-11-03 14:49_

This actually should error, now that I run it in `pip`. But we still need to support this locking behavior, because you can use the same Git URL with different subdirectory paths (which we don't yet support).

---

_@zanieb reviewed on 2023-11-03 14:55_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_git_concurrent_access.snap`:22 on 2023-11-03 14:55_

Okay that makes sense (#306 for ref)

We'll need to update this test later then?

---

_@charliermarsh reviewed on 2023-11-03 14:58_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__compile_git_concurrent_access.snap`:22 on 2023-11-03 14:58_

Yeah, I'll need to support subdirectory path before I can add a meaningful test, so I'll do that first (not too hard).

---

_@konstin reviewed on 2023-11-03 15:06_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:882 on 2023-11-03 15:06_

I've been thinking about having some kind of global waitmap, but i still have to prototype that

---

_@konstin approved on 2023-11-03 15:06_

---

_Merged by @charliermarsh on 2023-11-03 16:33_

---

_Closed by @charliermarsh on 2023-11-03 16:33_

---

_Branch deleted on 2023-11-03 16:33_

---
