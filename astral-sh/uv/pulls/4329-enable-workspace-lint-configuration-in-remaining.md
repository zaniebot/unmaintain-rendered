```yaml
number: 4329
title: Enable workspace lint configuration in remaining crates
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lints
created_at: 2024-06-14T15:58:59Z
updated_at: 2024-06-18T03:02:29Z
url: https://github.com/astral-sh/uv/pull/4329
synced_at: 2026-01-12T16:06:09Z
```

# Enable workspace lint configuration in remaining crates

---

_@charliermarsh_

## Summary

A change I did a while back but just pushing it before I lose context. We didn't have Clippy enabled (to match our workspace settings) in a few crates.


---

_Label `internal` added by @charliermarsh on 2024-06-14 15:59_

---

_@zanieb reviewed on 2024-06-14 18:30_

---

_Review comment by @zanieb on `crates/uv-client/src/httpcache/mod.rs`:129 on 2024-06-14 18:30_

```suggestion
* The 1997 RFC for HTTP 1.1: <https://www.rfc-editor.org/rfc/rfc2068#section-13>
```

---

_@zanieb approved on 2024-06-14 18:31_

---

_Comment by @zanieb on 2024-06-15 01:03_

@charliermarsh do you want me to get this cleaned up and merged?

---

_Comment by @charliermarsh on 2024-06-15 06:27_

If you don’t mind, I would appreciate it thanks.

---

_Comment by @charliermarsh on 2024-06-15 07:15_

But otherwise I will get to it later, it’s not urgent :)

---

_Assigned to @zanieb by @zanieb on 2024-06-15 13:59_

---

_Comment by @charliermarsh on 2024-06-17 10:24_

@zanieb - Actually, feel free to leave and I'll get to it when I'm back. It's peripheral and I shouldn't distract you with it.

---

_Merged by @zanieb on 2024-06-18 03:02_

---

_Closed by @zanieb on 2024-06-18 03:02_

---

_Branch deleted on 2024-06-18 03:02_

---
