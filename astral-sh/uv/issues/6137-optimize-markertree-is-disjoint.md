```yaml
number: 6137
title: "Optimize `MarkerTree::is_disjoint`"
type: issue
state: closed
author: ibraheemdev
labels:
  - performance
assignees: []
created_at: 2024-08-16T02:35:45Z
updated_at: 2024-08-16T17:03:27Z
url: https://github.com/astral-sh/uv/issues/6137
synced_at: 2026-01-12T15:59:01Z
```

# Optimize `MarkerTree::is_disjoint`

---

_@ibraheemdev_

We currently just call `MarkerTree::and`, which can cause unnecessary allocations because we only care if the result is satisfiable or not.

---

_Label `performance` added by @ibraheemdev on 2024-08-16 02:35_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2024-08-16 02:35_

---

_Closed by @ibraheemdev on 2024-08-16 17:03_

---

_Closed by @ibraheemdev on 2024-08-16 17:03_

---
