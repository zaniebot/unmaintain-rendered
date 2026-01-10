```yaml
number: 10989
title: "Forward SIGINT to children in `uv run` on second Ctrl-C"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/int
created_at: 2025-01-27T15:30:55Z
updated_at: 2025-01-28T17:29:38Z
url: https://github.com/astral-sh/uv/pull/10989
synced_at: 2026-01-10T11:45:21Z
```

# Forward SIGINT to children in `uv run` on second Ctrl-C

---

_Pull request opened by @zanieb on 2025-01-27 15:30_

Closes https://github.com/astral-sh/uv/issues/10952

---

_Label `bug` added by @zanieb on 2025-01-27 15:30_

---

_@zanieb reviewed on 2025-01-27 15:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:10 on 2025-01-27 15:31_

Need to figure out what this means for Windows before ready for review.

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:34 on 2025-01-27 15:32_

I'd like to understand why Ctrl-C isn't working in https://github.com/astral-sh/uv/issues/10952 but works in other cases, but... it seems generally correct to forward a SIGINT if we've received it multiple times (as an escape hatch).

---

_@zanieb reviewed on 2025-01-27 15:32_

---

_@zanieb reviewed on 2025-01-27 15:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:26 on 2025-01-27 15:32_

Keeping this separate for review purposes.

---

_Closed by @zanieb on 2025-01-28 17:29_

---
