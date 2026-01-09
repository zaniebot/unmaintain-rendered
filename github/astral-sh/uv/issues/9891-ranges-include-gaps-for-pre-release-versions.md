---
number: 9891
title: Ranges include gaps for pre-release versions
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-12-13T22:30:24Z
updated_at: 2024-12-17T17:05:17Z
url: https://github.com/astral-sh/uv/issues/9891
synced_at: 2026-01-07T13:12:18-06:00
---

# Ranges include gaps for pre-release versions

---

_Issue opened by @zanieb on 2024-12-13 22:30_

e.g., in https://github.com/astral-sh/uv/blob/4bce1a32ec49b8ae19a34e9a5b3e0dc65b399f62/crates/uv/tests/it/pip_install.rs#L2173-L2189

we show several ranges for `anyio` because there are disallowed pre-release versions in-between each range. We should just say "all versions" the whole time instead?

---

_Label `error messages` added by @zanieb on 2024-12-13 22:31_

---

_Referenced in [astral-sh/uv#9944](../../astral-sh/uv/pulls/9944.md) on 2024-12-16 21:37_

---

_Closed by @zanieb on 2024-12-17 17:05_

---

_Closed by @zanieb on 2024-12-17 17:05_

---
