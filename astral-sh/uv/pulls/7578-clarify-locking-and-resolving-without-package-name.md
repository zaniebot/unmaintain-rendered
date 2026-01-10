```yaml
number: 7578
title: Clarify locking and resolving without package name
type: pull_request
state: merged
author: lucab
labels:
  - internal
assignees: []
merged: true
base: main
head: ups/lock-comment
created_at: 2024-09-20T10:21:43Z
updated_at: 2024-10-02T13:35:14Z
url: https://github.com/astral-sh/uv/pull/7578
synced_at: 2026-01-10T12:53:50Z
```

# Clarify locking and resolving without package name

---

_Pull request opened by @lucab on 2024-09-20 10:21_

This adds a guiding comment around an hardcoded no-value in the lock-and-resolve flow.

Ref: https://github.com/astral-sh/uv/pull/7488#issuecomment-2358385418

---

_Label `internal` added by @lucab on 2024-09-20 10:21_

---

_Review requested from @zanieb by @lucab on 2024-09-20 10:21_

---

_Assigned to @zanieb by @zanieb on 2024-09-20 18:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:532 on 2024-10-01 18:47_

```suggestion
                // The root is always null in workspaces, it "depends on" the projects
```

---

_@zanieb reviewed on 2024-10-01 18:47_

---

_@zanieb approved on 2024-10-01 18:47_

---

_Merged by @zanieb on 2024-10-01 18:53_

---

_Closed by @zanieb on 2024-10-01 18:53_

---

_Branch deleted on 2024-10-02 13:35_

---
