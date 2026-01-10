```yaml
number: 5186
title: Log origin of version selection
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/log-preferences
created_at: 2024-07-18T13:23:59Z
updated_at: 2024-07-19T08:15:46Z
url: https://github.com/astral-sh/uv/pull/5186
synced_at: 2026-01-10T13:42:52Z
```

# Log origin of version selection

---

_Pull request opened by @konstin on 2024-07-18 13:23_

Log whether a version was picked because it was the next version or because it was a preference (installed, lockfile or sibling fork)

---

_Label `tracing` added by @konstin on 2024-07-18 13:24_

---

_@charliermarsh reviewed on 2024-07-19 00:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:432 on 2024-07-19 00:30_

Can we make this an enum?

---

_@charliermarsh approved on 2024-07-19 00:30_

---

_@charliermarsh reviewed on 2024-07-19 00:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1065 on 2024-07-19 00:31_

Maybe `[preference] ({})` so that the filename and preference are separate?

---

_Merged by @konstin on 2024-07-19 08:15_

---

_Closed by @konstin on 2024-07-19 08:15_

---

_Branch deleted on 2024-07-19 08:15_

---
