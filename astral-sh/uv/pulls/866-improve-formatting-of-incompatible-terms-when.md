```yaml
number: 866
title: Improve formatting of incompatible terms when there are two items
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/two-incompat
created_at: 2024-01-10T05:22:15Z
updated_at: 2024-01-10T20:36:55Z
url: https://github.com/astral-sh/uv/pull/866
synced_at: 2026-01-12T16:04:14Z
```

# Improve formatting of incompatible terms when there are two items

---

_@zanieb_

_No description provided._

---

_Label `error messages` added by @zanieb on 2024-01-10 05:30_

---

_@charliermarsh reviewed on 2024-01-10 16:01_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:165 on 2024-01-10 16:01_

Nit: I find the `.len().cmp()` a bit confusing vs. just using an `if` outside of the loop, but not blocking, up to you.

---

_@charliermarsh approved on 2024-01-10 16:01_

---

_@zanieb reviewed on 2024-01-10 16:13_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:165 on 2024-01-10 16:13_

I wrote the `if` but Clippy made me do this lol

---

_Merged by @zanieb on 2024-01-10 20:36_

---

_Closed by @zanieb on 2024-01-10 20:36_

---

_Branch deleted on 2024-01-10 20:36_

---
