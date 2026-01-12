```yaml
number: 14519
title: Drop trailing arguments when writing shebangs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/she
created_at: 2025-07-09T14:45:11Z
updated_at: 2025-07-09T15:51:08Z
url: https://github.com/astral-sh/uv/pull/14519
synced_at: 2026-01-12T16:11:16Z
```

# Drop trailing arguments when writing shebangs

---

_@charliermarsh_

## Summary

You can see in pip that they read the full first line, then replace it with the rewritten shebang, thereby dropping any trailing arguments on the shebang: https://github.com/pypa/pip/blob/65da0ff5349297da64ccadb4dd22ab41185ea0b9/src/pip/_internal/operations/install/wheel.py#L94

In contrast, we currently retain them, but write them _after_ the shebang, which is wrong.

Closes https://github.com/astral-sh/uv/issues/14470.


---

_Review requested from @konstin by @charliermarsh on 2025-07-09 14:45_

---

_Label `bug` added by @charliermarsh on 2025-07-09 14:45_

---

_Label `compatibility` added by @charliermarsh on 2025-07-09 14:45_

---

_Marked ready for review by @charliermarsh on 2025-07-09 14:46_

---

_@charliermarsh reviewed on 2025-07-09 14:48_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:434 on 2025-07-09 14:48_

It's pretty rare for there to be any trailing content here, so I think reading one-byte-at-a-time is probably fine. But we could also read (e.g.) 256 bytes into the buffer if desired.

---

_@zanieb approved on 2025-07-09 15:51_

---

_Merged by @zanieb on 2025-07-09 15:51_

---

_Closed by @zanieb on 2025-07-09 15:51_

---

_Branch deleted on 2025-07-09 15:51_

---
