```yaml
number: 3803
title: Remove extra details from interpreter query traces
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/fixup-1
created_at: 2024-05-23T21:18:41Z
updated_at: 2024-05-23T23:18:06Z
url: https://github.com/astral-sh/uv/pull/3803
synced_at: 2026-01-12T16:05:51Z
```

# Remove extra details from interpreter query traces

---

_@zanieb_

Cherry-picked from https://github.com/astral-sh/uv/pull/3797/commits/8f135c26f46c6fa84ac534a775c5212a0981690a


---

_Label `tracing` added by @zanieb on 2024-05-23 21:18_

---

_@zanieb reviewed on 2024-05-23 21:19_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:646 on 2024-05-23 21:19_

We log this in discovery, we don't need this redundant message here.

---

_@zanieb reviewed on 2024-05-23 21:21_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:654 on 2024-05-23 21:21_

Honestly a little confused by this but I think `?`was making it a debug representation which is way more verbose than we need?

---

_@charliermarsh approved on 2024-05-23 21:21_

---

_Merged by @zanieb on 2024-05-23 23:18_

---

_Closed by @zanieb on 2024-05-23 23:18_

---

_Branch deleted on 2024-05-23 23:18_

---
