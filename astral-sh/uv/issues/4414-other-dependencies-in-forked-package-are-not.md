---
number: 4414
title: Other dependencies in forked package are not filtered by fork markers
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-19T14:55:43Z
updated_at: 2024-06-20T11:21:46Z
url: https://github.com/astral-sh/uv/issues/4414
synced_at: 2026-01-10T01:23:37Z
---

# Other dependencies in forked package are not filtered by fork markers

---

_Issue opened by @konstin on 2024-06-19 14:55_

With the dependencies below, we fork on anyio over `python_version`. Currently, b1 and b2 are part of both splits, even though we know that the `anyio==4.4.0` split does not include b2 (and `anyio==4.3.0` doesn't include b1). This can cause unnecessary conflicts through the transitive dependencies of b1 and b2

```toml
dependencies = [
  # Force a split
  "anyio==4.4.0 ; python_version >= '3.12'",
  "anyio==4.3.0 ; python_version < '3.12'",
  "b1 ; python_version >= '3.12'",
  "b2 ; python_version < '3.12'",
]
```

---

_Label `bug` added by @konstin on 2024-06-19 14:55_

---

_Label `preview` added by @konstin on 2024-06-19 14:55_

---

_Assigned to @BurntSushi by @konstin on 2024-06-19 14:55_

---

_Referenced in [astral-sh/packse#197](../../astral-sh/packse/pulls/197.md) on 2024-06-19 15:23_

---

_Referenced in [astral-sh/uv#4415](../../astral-sh/uv/pulls/4415.md) on 2024-06-19 15:36_

---

_Closed by @BurntSushi on 2024-06-20 11:21_

---

_Closed by @BurntSushi on 2024-06-20 11:21_

---
