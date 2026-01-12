```yaml
number: 981
title: Avoid showing negations of ranges in error messages
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/negation-single
created_at: 2024-01-18T23:50:13Z
updated_at: 2024-01-19T17:07:16Z
url: https://github.com/astral-sh/uv/pull/981
synced_at: 2026-01-12T16:04:20Z
```

# Avoid showing negations of ranges in error messages

---

_@zanieb_

Closes https://github.com/astral-sh/puffin/issues/980


---

_Label `error messages` added by @zanieb on 2024-01-19 00:00_

---

_@charliermarsh approved on 2024-01-19 03:42_

---

_@zanieb reviewed on 2024-01-19 04:59_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:439 on 2024-01-19 04:59_

I actually don't think `set` is guaranteed to be a single segment here so the pull request title is misleading. Regardless, it seems better to use the complement than to negate a section. Worth keeping in mind when we start to see real-world errors.

---

_Renamed from "Avoid showing negations of single ranges in error messages" to "Avoid showing negations of ranges in error messages" by @zanieb on 2024-01-19 04:59_

---

_Merged by @zanieb on 2024-01-19 17:07_

---

_Closed by @zanieb on 2024-01-19 17:07_

---

_Branch deleted on 2024-01-19 17:07_

---
