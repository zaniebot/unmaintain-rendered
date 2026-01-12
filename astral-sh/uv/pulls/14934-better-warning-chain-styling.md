```yaml
number: 14934
title: " Better warning chain styling "
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: konsti/better-warning-chain
created_at: 2025-07-28T09:33:21Z
updated_at: 2025-07-28T16:23:40Z
url: https://github.com/astral-sh/uv/pull/14934
synced_at: 2026-01-12T16:11:30Z
```

#  Better warning chain styling 

---

_@konstin_

Improve the styling of warning chains for Python installation errors. Apply the same logic to other internal warning and error formatting locations.

**Before**

<img width="1232" height="364" alt="Screenshot from 2025-07-28 10-06-41" src="https://github.com/user-attachments/assets/e3befe14-ad4c-44ed-8b0a-57d9c9a3b815" />

**After**

<img width="1232" height="364" alt="Screenshot from 2025-07-28 10-23-49" src="https://github.com/user-attachments/assets/1bd890c1-5dbb-4662-93bd-14430c060a69" />


---

_Label `enhancement` added by @konstin on 2025-07-28 09:33_

---

_Label `error messages` added by @konstin on 2025-07-28 09:33_

---

_@zanieb reviewed on 2025-07-28 12:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:143 on 2025-07-28 12:36_

Why this change? 

---

_@zanieb approved on 2025-07-28 12:52_

---

_@konstin reviewed on 2025-07-28 12:59_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:143 on 2025-07-28 12:59_

Previously, we would not check the windows formatting paths on Unix, making it harder to work with this code. Preferable, `Registry` would actually be used and the path would be `if cfg!(windows)`'d, but that doesn't work with the platform specific dependencies in the `windows_registry` module.

---

_Merged by @konstin on 2025-07-28 16:23_

---

_Closed by @konstin on 2025-07-28 16:23_

---

_Branch deleted on 2025-07-28 16:23_

---
