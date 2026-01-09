---
number: 14878
title: "Reject unnamed indexes with `explicit = true`"
type: issue
state: open
author: charliermarsh
labels:
  - error messages
  - breaking
assignees: []
created_at: 2025-07-24T20:51:51Z
updated_at: 2026-01-02T08:17:15Z
url: https://github.com/astral-sh/uv/issues/14878
synced_at: 2026-01-07T13:12:19-06:00
---

# Reject unnamed indexes with `explicit = true`

---

_Issue opened by @charliermarsh on 2025-07-24 20:51_

E.g., this index could never be used:

```toml
[[tool.uv.index]]
url = "..."
explicit = true
```

This would be a breaking change.

---

_Label `error messages` added by @charliermarsh on 2025-07-24 20:51_

---

_Label `breaking` added by @charliermarsh on 2025-07-24 20:51_

---

_Added to milestone `v0.9.0` by @zanieb on 2025-07-24 21:00_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:16_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:16_

---
