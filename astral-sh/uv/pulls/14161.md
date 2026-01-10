```yaml
number: 14161
title: "Remove `uv version` fallback"
type: pull_request
state: merged
author: Gankra
labels:
  - breaking
assignees: []
merged: true
base: release/080
head: gankra/remove-vfall
created_at: 2025-06-20T15:44:34Z
updated_at: 2025-07-16T13:24:07Z
url: https://github.com/astral-sh/uv/pull/14161
synced_at: 2026-01-10T06:53:01Z
```

# Remove `uv version` fallback

---

_Pull request opened by @Gankra on 2025-06-20 15:44_

Fixes #14157 

---

_Added to milestone `v0.8.0` by @Gankra on 2025-06-20 15:44_

---

_Label `breaking` added by @Gankra on 2025-06-20 15:44_

---

_@zanieb approved on 2025-06-21 19:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:1443 on 2025-07-14 14:22_

Nit: It's not deprecated, it's removed now — deprecated is when we're warning that the behavior will change in the future.

While we're here we should change this to a doc comment.



---

_@zanieb reviewed on 2025-07-14 14:22_

---

_@zanieb approved on 2025-07-14 14:23_

---

_Merged by @zanieb on 2025-07-16 13:24_

---

_Closed by @zanieb on 2025-07-16 13:24_

---

_Branch deleted on 2025-07-16 13:24_

---
