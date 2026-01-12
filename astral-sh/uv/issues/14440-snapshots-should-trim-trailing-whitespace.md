```yaml
number: 14440
title: Snapshots should trim trailing whitespace
type: issue
state: open
author: konstin
labels:
  - internal
assignees: []
created_at: 2025-07-03T12:24:30Z
updated_at: 2025-07-03T17:15:50Z
url: https://github.com/astral-sh/uv/issues/14440
synced_at: 2026-01-12T16:01:49Z
```

# Snapshots should trim trailing whitespace

---

_@konstin_

With an editor set to trim trailing whitespace, the following line causes a test failure, as it's empty except whitespace:

https://github.com/astral-sh/uv/blob/a6bb65c78deaf6fef7b901d4d24d350368db2723/crates/uv/tests/it/tool_install.rs#L1717

We should normalize snapshots to trim whitespace.

---

_Label `internal` added by @konstin on 2025-07-03 12:24_

---

_Label `internal` added by @konstin on 2025-07-03 12:24_

---

_Comment by @zanieb on 2025-07-03 12:28_

Shouldn't we be emitting that without trailing whitspace?

---

_Comment by @konstin on 2025-07-03 15:33_

It's not our error message, but an external one, forwarding those verbatim seems correct.

---

_Comment by @zanieb on 2025-07-03 15:57_

Hm okay. It also seems proper to have snapshots reflect the literal output, but 🤷‍♀ 

---

_Comment by @konstin on 2025-07-03 17:15_

We can install a different package instead if that helps

---
