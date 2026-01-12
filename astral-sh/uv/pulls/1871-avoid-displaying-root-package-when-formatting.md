```yaml
number: 1871
title: "Avoid displaying \"root\" package when formatting terms"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/root-incompat
created_at: 2024-02-22T14:57:10Z
updated_at: 2024-02-22T18:04:20Z
url: https://github.com/astral-sh/uv/pull/1871
synced_at: 2026-01-12T16:04:45Z
```

# Avoid displaying "root" package when formatting terms

---

_@zanieb_

We don't have test coverage for this, but a term can reference an incompatibility with root and then we'll display the internal 'root' package to the user.

Raised in https://github.com/astral-sh/uv/issues/1855

---

_Label `error messages` added by @zanieb on 2024-02-22 14:57_

---

_@charliermarsh reviewed on 2024-02-22 16:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:741 on 2024-02-22 16:40_

Is the underscore here interntional?

---

_@zanieb reviewed on 2024-02-22 17:59_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:741 on 2024-02-22 17:59_

Nope dunno how that happened

---

_Merged by @zanieb on 2024-02-22 18:04_

---

_Closed by @zanieb on 2024-02-22 18:04_

---

_Branch deleted on 2024-02-22 18:04_

---
