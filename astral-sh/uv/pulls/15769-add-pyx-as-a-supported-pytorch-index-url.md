```yaml
number: 15769
title: Add pyx as a supported PyTorch index URL
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pyx-torch
created_at: 2025-09-10T16:55:03Z
updated_at: 2025-09-10T19:38:02Z
url: https://github.com/astral-sh/uv/pull/15769
synced_at: 2026-01-10T06:36:15Z
```

# Add pyx as a supported PyTorch index URL

---

_Pull request opened by @charliermarsh on 2025-09-10 16:55_

## Summary

If the user explicitly authenticated to pyx, then we attempt to use the pyx PyTorch URLs; otherwise, we stick to `download.pytorch.org` as the default.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-10 16:55_

---

_Marked ready for review by @charliermarsh on 2025-09-10 18:40_

---

_@zanieb reviewed on 2025-09-10 18:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/install.rs`:374 on 2025-09-10 18:48_

nit: This feels like it could be `.then(...).unwrap_or_default()` but I don't really mind either pattern

---

_@zanieb reviewed on 2025-09-10 18:54_

---

_Review comment by @zanieb on `crates/uv-torch/src/backend.rs`:1113 on 2025-09-10 18:54_

Can we call this `PYX_API_BASE_URL`?

---

_@zanieb reviewed on 2025-09-10 18:54_

---

_Review comment by @zanieb on `crates/uv-torch/src/backend.rs`:716 on 2025-09-10 18:54_

I guess we should add this to the todo-list for a `is_known_url` refactor since there's overlap

---

_@zanieb approved on 2025-09-10 18:54_

---

_Merged by @zanieb on 2025-09-10 19:38_

---

_Closed by @zanieb on 2025-09-10 19:38_

---

_Branch deleted on 2025-09-10 19:38_

---
