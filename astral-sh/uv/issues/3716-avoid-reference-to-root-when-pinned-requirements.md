---
number: 3716
title: Avoid reference to root when pinned requirements are incompatible
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-05-21T18:51:29Z
updated_at: 2024-05-21T18:52:17Z
url: https://github.com/astral-sh/uv/issues/3716
synced_at: 2026-01-10T01:23:30Z
---

# Avoid reference to root when pinned requirements are incompatible

---

_Issue opened by @zanieb on 2024-05-21 18:51_

See https://github.com/astral-sh/uv/pull/3711

Instead of 

```
× No solution found when resolving dependencies:
╰─▶ Because flask==3.0.2 depends on click>=8.1.3 and you require click==7.0.0, we can conclude that your requirements and flask==3.0.2 are incompatible.
    And because you require flask==3.0.2, we can conclude that the requirements are unsatisfiable.
```

We should say

```
× No solution found when resolving dependencies:
╰─▶ Because flask==3.0.2 depends on click>=8.1.3 and you require click==7.0.0 and flask==3.0.2, we can conclude that the requirements are unsatisfiable.
```

---

_Label `error messages` added by @zanieb on 2024-05-21 18:51_

---

_Referenced in [astral-sh/uv#3711](../../astral-sh/uv/pulls/3711.md) on 2024-05-21 18:52_

---
