```yaml
number: 9553
title: "Use `SharedState` for build dispatch"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/shared-state
created_at: 2024-12-01T13:11:29Z
updated_at: 2024-12-01T22:20:31Z
url: https://github.com/astral-sh/uv/pull/9553
synced_at: 2026-01-10T12:00:00Z
```

# Use `SharedState` for build dispatch

---

_Pull request opened by @konstin on 2024-12-01 13:11_

When looking at the build frontend code, I noticed that we always pass every single field of the shared state to the build dispatch:

```rust
    let build_dispatch = BuildDispatch::new(
        ...
        &state.index,
        &state.git,
        &state.capabilities,
        &state.in_flight,
        ...
    );
```

We can abstract this by moving `SharedState` into the build dispatch. The `BuildDispatch` then has only immutable fields and the `SharedState`. Since the `SharedState` is all `Arc`s, we can clone it freely.


---

_Review requested from @charliermarsh by @konstin on 2024-12-01 13:11_

---

_Label `internal` added by @konstin on 2024-12-01 13:11_

---

_@charliermarsh approved on 2024-12-01 13:44_

---

_Merged by @charliermarsh on 2024-12-01 22:20_

---

_Closed by @charliermarsh on 2024-12-01 22:20_

---

_Branch deleted on 2024-12-01 22:20_

---
