```yaml
number: 3275
title: No logging from rayon threads
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-04-26T09:39:06Z
updated_at: 2024-04-28T19:54:20Z
url: https://github.com/astral-sh/uv/issues/3275
synced_at: 2026-01-12T15:58:42Z
```

# No logging from rayon threads

---

_@konstin_

We run wheel installation instead of tokio threads for faster performance, implemented [here](https://github.com/astral-sh/uv/blob/725004dcf12c2d9c73aab7020c4fba92a6672d88/crates/uv-installer/src/installer.rs#L50-L78). I don't see any log messages or spans from anything that happens inside these threads, both with `-v` and with `-vv`. `println!` is shown correctly. We should make sure log messages are correctly forwarded.

---

_Label `bug` added by @konstin on 2024-04-26 09:39_

---

_Comment by @konstin on 2024-04-28 19:54_

Fixed by #3293

---

_Closed by @konstin on 2024-04-28 19:54_

---
