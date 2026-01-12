```yaml
number: 4222
title: Update test context to avoid discovery of external Pythons
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/no-pythons
created_at: 2024-06-10T21:46:41Z
updated_at: 2024-06-10T22:26:54Z
url: https://github.com/astral-sh/uv/pull/4222
synced_at: 2026-01-12T16:06:06Z
```

# Update test context to avoid discovery of external Pythons

---

_@zanieb_

By setting the test search path to an empty path, we avoid accidentally pulling interpreters from the system during a test case.

Cherry-picked from https://github.com/astral-sh/uv/pull/4214

---

_Label `testing` added by @zanieb on 2024-06-10 21:46_

---

_Marked ready for review by @zanieb on 2024-06-10 21:47_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-10 22:01_

---

_Review requested from @konstin by @zanieb on 2024-06-10 22:01_

---

_@charliermarsh reviewed on 2024-06-10 22:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/common/mod.rs`:304 on 2024-06-10 22:20_

Should we use empty string? Would it be more portable?

---

_@charliermarsh approved on 2024-06-10 22:20_

---

_@zanieb reviewed on 2024-06-10 22:22_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:304 on 2024-06-10 22:22_

I was worried we might treat an empty string as an empty variable but I guess we don't after looking at the source. Since this passes on Windows CI I think I'd prefer to leave it as-is because the intent is clearer? 🤷‍♀️ 

---

_Merged by @zanieb on 2024-06-10 22:26_

---

_Closed by @zanieb on 2024-06-10 22:26_

---

_Branch deleted on 2024-06-10 22:26_

---
