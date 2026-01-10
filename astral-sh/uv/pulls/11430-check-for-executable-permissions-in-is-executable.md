```yaml
number: 11430
title: "Check for executable permissions in `is_executable`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/is-executable
created_at: 2025-02-11T21:53:45Z
updated_at: 2025-02-12T19:50:32Z
url: https://github.com/astral-sh/uv/pull/11430
synced_at: 2026-01-10T11:10:38Z
```

# Check for executable permissions in `is_executable`

---

_Pull request opened by @zanieb on 2025-02-11 21:53_

e.g., as in https://docs.rs/is_executable/latest/src/is_executable/lib.rs.html#44

---

_Label `bug` added by @zanieb on 2025-02-11 21:53_

---

_@charliermarsh reviewed on 2025-02-11 22:26_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/which.rs`:32 on 2025-02-11 22:26_

I think `permissions` isn't defined here.


---

_Review comment by @zanieb on `crates/uv-fs/src/which.rs`:32 on 2025-02-11 22:31_

Ah thanks... bad me

---

_@zanieb reviewed on 2025-02-11 22:31_

---

_Marked ready for review by @zanieb on 2025-02-12 16:00_

---

_Merged by @zanieb on 2025-02-12 19:50_

---

_Closed by @zanieb on 2025-02-12 19:50_

---

_Branch deleted on 2025-02-12 19:50_

---
