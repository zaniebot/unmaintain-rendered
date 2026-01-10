```yaml
number: 11290
title: "Allow `--python <dir>` requests to match existing environments if `sys.executable` is the same file"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/find-dir
created_at: 2025-02-06T19:33:06Z
updated_at: 2025-02-12T18:46:02Z
url: https://github.com/astral-sh/uv/pull/11290
synced_at: 2026-01-10T11:10:35Z
```

# Allow `--python <dir>` requests to match existing environments if `sys.executable` is the same file

---

_Pull request opened by @zanieb on 2025-02-06 19:33_

Closes https://github.com/astral-sh/uv/issues/11288

I tested the reproduction there manually.

I'm a little uncertain about this behavior, it's not true to the spirit of `--python <dir>` selecting a target environment but this method is only used to see if an existing environment matches for the purpose of invalidation in projects and tools where I think we always force a separate environment anyway?

---

_Label `bug` added by @zanieb on 2025-02-06 19:33_

---

_@zanieb reviewed on 2025-02-07 20:02_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1480 on 2025-02-07 20:02_

Arguably, we could remove this first check. Mildly afraid of something breaking though.

---

_Marked ready for review by @zanieb on 2025-02-09 18:35_

---

_@konstin approved on 2025-02-10 11:57_

---

_Merged by @zanieb on 2025-02-12 18:46_

---

_Closed by @zanieb on 2025-02-12 18:46_

---

_Branch deleted on 2025-02-12 18:46_

---
