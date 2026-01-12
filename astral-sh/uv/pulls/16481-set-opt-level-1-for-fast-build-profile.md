```yaml
number: 16481
title: Set opt-level 1 for fast build profile
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fast-build-opt-level-1
created_at: 2025-10-28T12:04:31Z
updated_at: 2025-10-28T12:31:20Z
url: https://github.com/astral-sh/uv/pull/16481
synced_at: 2026-01-12T16:12:16Z
```

# Set opt-level 1 for fast build profile

---

_@konstin_

Test cases:

```
touch crates/uv-resolver/src/resolver/mod.rs && time cargo nextest run --cargo-profile dev --no-fail-fast -- --skip python_install
touch crates/uv-resolver/src/resolver/mod.rs && time cargo nextest run --cargo-profile fast-build --no-fail-fast -- --skip python_install
```

On my machine, we go from 7.x s (dev) to 8.x s (dev + opt-level 1) for compilation, and 6.x s for the combined `fast-build` profile. With opt-level 1, we go from 84s for the first line to 64s for the second line.


---

_Label `internal` added by @konstin on 2025-10-28 12:04_

---

_@zanieb approved on 2025-10-28 12:13_

---

_Merged by @konstin on 2025-10-28 12:31_

---

_Closed by @konstin on 2025-10-28 12:31_

---

_Branch deleted on 2025-10-28 12:31_

---
