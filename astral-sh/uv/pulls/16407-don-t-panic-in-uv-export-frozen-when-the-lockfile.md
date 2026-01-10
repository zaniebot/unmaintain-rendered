```yaml
number: 16407
title: "Don't panic in `uv export --frozen` when the lockfile is outdated"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-panic-when-the-lockfile-is-outdated
created_at: 2025-10-22T10:47:58Z
updated_at: 2025-10-23T20:07:15Z
url: https://github.com/astral-sh/uv/pull/16407
synced_at: 2026-01-10T06:36:16Z
```

# Don't panic in `uv export --frozen` when the lockfile is outdated

---

_Pull request opened by @konstin on 2025-10-22 10:47_

Provide a good error message when the discovered workspace members mismatch with the locked workspace members in `uv export --frozen`, instead of panicking.

Fixes #16406

---

_Label `bug` added by @konstin on 2025-10-22 10:47_

---

_@konstin reviewed on 2025-10-22 10:49_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/mod.rs`:82 on 2025-10-22 10:49_

We now catch this early, but there's no reason to panic with there's different paths to accessing this code.

---

_@konstin reviewed on 2025-10-22 10:50_

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:91 on 2025-10-22 10:50_

The missing workspace member is only for our debugging, we could also put this in a nested source.

---

_@konstin reviewed on 2025-10-22 11:27_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:347 on 2025-10-22 11:27_

This is technically more strict than the previous check, but it will only fail in cases where the lockfile doesn't match the on-disk workspace structure, while afaik we require the workspace structure to be intact through `VirtualProject::discover` (`uv export --frozen --no-emit-workspace --no-emit-project` panics too)?

---

_@zanieb approved on 2025-10-23 20:07_

---

_Merged by @zanieb on 2025-10-23 20:07_

---

_Closed by @zanieb on 2025-10-23 20:07_

---

_Branch deleted on 2025-10-23 20:07_

---
