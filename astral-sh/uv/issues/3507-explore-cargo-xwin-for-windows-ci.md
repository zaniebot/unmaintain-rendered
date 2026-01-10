---
number: 3507
title: Explore cargo-xwin for windows CI
type: issue
state: closed
author: konstin
labels:
  - internal
  - windows
assignees: []
created_at: 2024-05-10T14:39:02Z
updated_at: 2024-05-17T02:15:04Z
url: https://github.com/astral-sh/uv/issues/3507
synced_at: 2026-01-10T01:23:28Z
---

# Explore cargo-xwin for windows CI

---

_Issue opened by @konstin on 2024-05-10 14:39_

Our windows CI is currently slow and inconsistent in performance. We could run e.g. the windows clippy job with cargo-xwin and see if we can speed up at least the feedback cycle on clippy:

```
cargo xwin clippy --target x86_64-pc-windows-msvc --workspace --all-targets --all-features --locked --profile fast-build -- -D warnings
```

---

_Label `internal` added by @charliermarsh on 2024-05-10 18:40_

---

_Label `windows` added by @charliermarsh on 2024-05-10 18:40_

---

_Referenced in [astral-sh/uv#3522](../../astral-sh/uv/pulls/3522.md) on 2024-05-11 15:21_

---

_Referenced in [astral-sh/uv#3600](../../astral-sh/uv/pulls/3600.md) on 2024-05-15 03:19_

---

_Closed by @zanieb on 2024-05-17 02:15_

---
