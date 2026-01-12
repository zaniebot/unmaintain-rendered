```yaml
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
synced_at: 2026-01-12T16:00:01Z
```

# Ranges include gaps for pre-release versions

---

_@zanieb_

e.g., in https://github.com/astral-sh/uv/blob/4bce1a32ec49b8ae19a34e9a5b3e0dc65b399f62/crates/uv/tests/it/pip_install.rs#L2173-L2189

we show several ranges for `anyio` because there are disallowed pre-release versions in-between each range. We should just say "all versions" the whole time instead?

---

_Label `error messages` added by @zanieb on 2024-12-13 22:31_

---

_Closed by @zanieb on 2024-12-17 17:05_

---

_Closed by @zanieb on 2024-12-17 17:05_

---
