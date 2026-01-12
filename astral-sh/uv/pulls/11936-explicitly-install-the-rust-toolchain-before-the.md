```yaml
number: 11936
title: Explicitly install the rust toolchain before the target during Docker builds
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/fix-docker
created_at: 2025-03-03T23:08:32Z
updated_at: 2025-03-04T14:23:07Z
url: https://github.com/astral-sh/uv/pull/11936
synced_at: 2026-01-12T16:10:04Z
```

# Explicitly install the rust toolchain before the target during Docker builds

---

_@zanieb_

Fixes https://github.com/astral-sh/uv/actions/runs/13641331357/job/38131724427

---

_Label `releases` added by @zanieb on 2025-03-03 23:08_

---

_Merged by @zanieb on 2025-03-03 23:17_

---

_Closed by @zanieb on 2025-03-03 23:17_

---

_Branch deleted on 2025-03-03 23:17_

---

_Comment by @zanieb on 2025-03-04 14:23_

Broke in

- https://github.com/rust-lang/rustup/pull/3985
- https://github.com/rust-lang/rustup/issues/4211

---
