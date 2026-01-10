```yaml
number: 10639
title: "Avoid reading symlinks during `uv python install` on Windows"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-link-win
created_at: 2025-01-15T18:59:56Z
updated_at: 2025-01-15T20:19:00Z
url: https://github.com/astral-sh/uv/pull/10639
synced_at: 2026-01-10T11:45:01Z
```

# Avoid reading symlinks during `uv python install` on Windows

---

_Pull request opened by @zanieb on 2025-01-15 18:59_

Closes #10633


---

_Label `bug` added by @zanieb on 2025-01-15 18:59_

---

_Review requested from @Gankra by @zanieb on 2025-01-15 19:00_

---

_Review requested from @konstin by @zanieb on 2025-01-15 19:01_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:376 on 2025-01-15 19:59_

Can this be `cfg!(unix) || target.read_link()`?

---

_@konstin approved on 2025-01-15 20:13_

---

_@zanieb reviewed on 2025-01-15 20:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:376 on 2025-01-15 20:16_

Wouldn't that be wrong? i.e., it'd be `true || ...` and we wouldn't do the latter invocation? We could do `cfg!(windows) || ...` though.

---

_Merged by @zanieb on 2025-01-15 20:17_

---

_Closed by @zanieb on 2025-01-15 20:17_

---

_Branch deleted on 2025-01-15 20:17_

---

_@zanieb reviewed on 2025-01-15 20:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:376 on 2025-01-15 20:19_

https://github.com/astral-sh/uv/pull/10642

---
