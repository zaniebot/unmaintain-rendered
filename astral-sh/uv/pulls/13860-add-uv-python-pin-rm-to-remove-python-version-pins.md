```yaml
number: 13860
title: "Add `uv python pin --rm` to remove `.python-version` pins"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/pin-rm
created_at: 2025-06-05T14:47:58Z
updated_at: 2025-06-05T17:05:44Z
url: https://github.com/astral-sh/uv/pull/13860
synced_at: 2026-01-10T11:10:42Z
```

# Add `uv python pin --rm` to remove `.python-version` pins

---

_Pull request opened by @zanieb on 2025-06-05 14:47_

I realized it is non-trivial to delete the global Python version pin, and that we were missing this simple functionality

---

_Label `enhancement` added by @zanieb on 2025-06-05 14:47_

---

_Renamed from "zb/pin rm" to "Add `uv python pin --rm` to remove `.python-version` pins" by @zanieb on 2025-06-05 14:48_

---

_Review requested from @Gankra by @zanieb on 2025-06-05 16:04_

---

_Assigned to @Gankra by @zanieb on 2025-06-05 16:04_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:68 on 2025-06-05 16:23_

nit: since this is trailing we don't "need" the backticks (but it's more uniform with other print outs in this command to have them?)

---

_@Gankra approved on 2025-06-05 16:23_

---

_@zanieb reviewed on 2025-06-05 17:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:68 on 2025-06-05 17:05_

I actually tried "Removed Python version file: {}" but it was awkward since the common case is a version file in the working directory so I reverted it.

---

_Merged by @zanieb on 2025-06-05 17:05_

---

_Closed by @zanieb on 2025-06-05 17:05_

---

_Branch deleted on 2025-06-05 17:05_

---
