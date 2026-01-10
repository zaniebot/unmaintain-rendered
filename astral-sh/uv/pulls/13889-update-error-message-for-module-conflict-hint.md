```yaml
number: 13889
title: Update error message for module conflict hint
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: konsti/warn-on-module-conflicts
head: zb/conflict-hint-review
created_at: 2025-06-06T19:04:56Z
updated_at: 2025-06-11T22:33:41Z
url: https://github.com/astral-sh/uv/pull/13889
synced_at: 2026-01-10T11:10:42Z
```

# Update error message for module conflict hint

---

_Pull request opened by @zanieb on 2025-06-06 19:04_

Review for https://github.com/astral-sh/uv/pull/13437

---

_@zanieb reviewed on 2025-06-06 19:10_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/linker.rs`:47 on 2025-06-06 19:10_

Discussion on inclusion of the wheel filename is happening at https://github.com/astral-sh/uv/pull/13437#discussion_r2132744173

---

_Assigned to @konstin by @zanieb on 2025-06-09 22:32_

---

_@konstin reviewed on 2025-06-11 19:34_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/linker.rs`:47 on 2025-06-11 19:34_

The error should by itself contain enough information to track down the problem, so I would like it to at least contain the versions.

---

_@zanieb reviewed on 2025-06-11 20:06_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/linker.rs`:47 on 2025-06-11 20:06_

👍 I'm happy to hand this off to you, unless you want me to write the version display.

---

_Comment by @zanieb on 2025-06-11 22:33_

I believe this was incorporated.

---

_Closed by @zanieb on 2025-06-11 22:33_

---
