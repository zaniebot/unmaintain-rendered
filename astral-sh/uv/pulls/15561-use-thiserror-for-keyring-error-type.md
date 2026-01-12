```yaml
number: 15561
title: "Use `thiserror` for keyring error type"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/keyring-thiserror
created_at: 2025-08-27T18:49:00Z
updated_at: 2025-08-28T13:09:13Z
url: https://github.com/astral-sh/uv/pull/15561
synced_at: 2026-01-12T16:11:49Z
```

# Use `thiserror` for keyring error type

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2025-08-27 18:49_

---

_Review comment by @zanieb on `crates/uv-keyring/src/error.rs`:67 on 2025-08-27 18:49_

The original motivation here was actually that it was annoying that we shared the inner error here twice when displaying chains to the user.

---

_@zanieb reviewed on 2025-08-27 18:49_

---

_@charliermarsh approved on 2025-08-28 01:02_

---

_@konstin approved on 2025-08-28 09:23_

---

_Merged by @zanieb on 2025-08-28 13:09_

---

_Closed by @zanieb on 2025-08-28 13:09_

---

_Branch deleted on 2025-08-28 13:09_

---
