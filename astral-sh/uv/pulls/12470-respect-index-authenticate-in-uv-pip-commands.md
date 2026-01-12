```yaml
number: 12470
title: "Respect `index.authenticate` in `uv pip` commands "
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/auth-test
created_at: 2025-03-25T18:56:45Z
updated_at: 2025-04-01T18:49:08Z
url: https://github.com/astral-sh/uv/pull/12470
synced_at: 2026-01-12T16:10:17Z
```

# Respect `index.authenticate` in `uv pip` commands 

---

_@jtfmumm_

We were not applying the `authenticate = "always"` behavior to `uv pip` commands (related to #12362). This PR addresses that, applying authentication policies wherever we set up a registry client.


---

_Label `bug` added by @jtfmumm on 2025-03-25 18:56_

---

_Marked ready for review by @jtfmumm on 2025-03-25 18:57_

---

_Review requested from @zanieb by @jtfmumm on 2025-03-25 18:57_

---

_Review comment by @konstin on `crates/uv-auth/src/middleware.rs`:187 on 2025-03-25 19:55_

Can you join these two log messages?

---

_Comment by @zanieb on 2025-03-25 20:03_

Could you update the title to reflect the user-facing change?

---

_@konstin approved on 2025-03-25 20:10_

---

_Renamed from "Add tracing for AuthPolicy" to "Add auth policy support for pip commands " by @jtfmumm on 2025-03-25 20:25_

---

_@jtfmumm reviewed on 2025-03-25 20:58_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:187 on 2025-03-25 20:58_

Updated

---

_Merged by @jtfmumm on 2025-03-25 21:13_

---

_Closed by @jtfmumm on 2025-03-25 21:13_

---

_Branch deleted on 2025-03-25 21:13_

---

_Renamed from "Add auth policy support for pip commands " to "Respect `index.authenticate` in `uv pip` commands " by @zanieb on 2025-04-01 18:49_

---
