---
number: 5366
title: "`uv` project and tool APIs are missing index credential extraction"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
  - preview
assignees: []
created_at: 2024-07-23T19:12:46Z
updated_at: 2024-07-23T20:33:54Z
url: https://github.com/astral-sh/uv/issues/5366
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv` project and tool APIs are missing index credential extraction

---

_Issue opened by @charliermarsh on 2024-07-23 19:12_

This code:

```rust
// Add all authenticated sources to the cache.
for url in index_locations.urls() {
    store_credentials_from_url(url);
}
```

Can't remember if it's necessary for correctness or just an optimization.


---

_Label `internal` added by @charliermarsh on 2024-07-23 19:12_

---

_Label `preview` added by @charliermarsh on 2024-07-23 19:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-23 20:01_

---

_Referenced in [astral-sh/uv#5367](../../astral-sh/uv/pulls/5367.md) on 2024-07-23 20:18_

---

_Closed by @charliermarsh on 2024-07-23 20:33_

---

_Closed by @charliermarsh on 2024-07-23 20:33_

---
