```yaml
number: 7437
title: "Add test case for Python pre-release versions from `VIRTUAL_ENV`"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/test-virtual-env-pre
created_at: 2024-09-16T19:51:22Z
updated_at: 2024-09-16T20:41:55Z
url: https://github.com/astral-sh/uv/pull/7437
synced_at: 2026-01-12T16:07:49Z
```

# Add test case for Python pre-release versions from `VIRTUAL_ENV`

---

_@zanieb_

_No description provided._

---

_Label `testing` added by @zanieb on 2024-09-16 19:51_

---

_Review comment by @henryiii on `crates/uv-python/src/lib.rs`:992 on 2024-09-16 19:56_

Would it be a good idea not to call this `.venv`, to make sure it doesn't pick it up that way? Or is this guaranteed to be in a different directory?

---

_@henryiii reviewed on 2024-09-16 19:56_

---

_@zanieb reviewed on 2024-09-16 20:04_

---

_Review comment by @zanieb on `crates/uv-python/src/lib.rs`:992 on 2024-09-16 20:04_

Yeah that's a good point, though this is just copied from an old test case. I'll update both.

---

_Merged by @zanieb on 2024-09-16 20:41_

---

_Closed by @zanieb on 2024-09-16 20:41_

---

_Branch deleted on 2024-09-16 20:41_

---
