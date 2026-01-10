```yaml
number: 14122
title: "Use Depot for Windows `cargo test`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/depot-win
created_at: 2025-06-18T02:15:47Z
updated_at: 2025-06-18T14:55:10Z
url: https://github.com/astral-sh/uv/pull/14122
synced_at: 2026-01-10T11:10:42Z
```

# Use Depot for Windows `cargo test`

---

_Pull request opened by @zanieb on 2025-06-18 02:15_

Replaces https://github.com/astral-sh/uv/pull/12320

Switches to Depot for the large Windows runner we use for `cargo test`.

The runtime goes from 8m 20s -> 6m 44s (total) and 7m 18s -> 4m 41s (test run) which are 20% and 35% speedups respectively.

A few things got marginally slower, like Python installs went from 11s -> 38s, the Rust cache went from 15s -> 30s, and drive setup went from 7s -> 20s.

---

_Label `internal` added by @zanieb on 2025-06-18 02:15_

---

_@samypr100 reviewed on 2025-06-18 03:20_

---

_Review comment by @samypr100 on `.github/workflows/setup-dev-drive.ps1`:18 on 2025-06-18 03:20_

> we use `V:` as the target instead.

Nice 😆 

---

_Marked ready for review by @zanieb on 2025-06-18 12:04_

---

_Assigned to @Gankra by @zanieb on 2025-06-18 14:36_

---

_Unassigned @Gankra by @zanieb on 2025-06-18 14:36_

---

_Review requested from @Gankra by @zanieb on 2025-06-18 14:36_

---

_Review requested from @Gankra by @zanieb on 2025-06-18 14:36_

---

_@Gankra approved on 2025-06-18 14:47_

wild

---

_Merged by @zanieb on 2025-06-18 14:55_

---

_Closed by @zanieb on 2025-06-18 14:55_

---

_Branch deleted on 2025-06-18 14:55_

---
