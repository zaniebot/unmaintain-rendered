```yaml
number: 15748
title: Show a dedicated error for venvs in source trees
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - error messages
  - build-backend
assignees: []
merged: true
base: main
head: konsti/dedicated-venv-in-uv-build-error
created_at: 2025-09-09T11:35:20Z
updated_at: 2025-09-09T12:41:59Z
url: https://github.com/astral-sh/uv/pull/15748
synced_at: 2026-01-10T11:10:46Z
```

# Show a dedicated error for venvs in source trees

---

_Pull request opened by @konstin on 2025-09-09 11:35_

A user in the support chat had an error message for `uv build` with the `uv_build` backend they didn't understand, which was caused by them having a venv in their build directory. This PR adds a dedicated error message when adding something to a distribution that looks like a venv.

---

_Label `enhancement` added by @konstin on 2025-09-09 11:35_

---

_Label `error messages` added by @konstin on 2025-09-09 11:35_

---

_Label `build-backend` added by @konstin on 2025-09-09 11:35_

---

_@zanieb reviewed on 2025-09-09 11:40_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/lib.rs`:360 on 2025-09-09 11:40_

dbg

---

_@zanieb approved on 2025-09-09 11:42_

---

_Merged by @konstin on 2025-09-09 12:41_

---

_Closed by @konstin on 2025-09-09 12:41_

---

_Branch deleted on 2025-09-09 12:41_

---
