```yaml
number: 1348
title: Display variance on hover over type variables
type: issue
state: closed
author: mtshiba
labels:
  - server
assignees: []
created_at: 2025-10-13T17:09:44Z
updated_at: 2025-10-20T17:28:38Z
url: https://github.com/astral-sh/ty/issues/1348
synced_at: 2026-01-10T02:06:25Z
```

# Display variance on hover over type variables

---

_Issue opened by @mtshiba on 2025-10-13 17:09_

The variance of PEP-695 style type variables is inferred based on how they are used.
It would be useful to be able to see the inferred variance in the editor by hovering over the type variable.

This feature is implemented in pyright/pylance.

<img width="753" height="145" alt="Image" src="https://github.com/user-attachments/assets/bdc322b7-64ee-4cb8-b41b-8c80a6b79678" />

By the way, in the playground, nothing seems to be displayed even if we try to hover over the type variable definition of a class.

---

_Label `server` added by @AlexWaygood on 2025-10-13 17:10_

---

_Closed by @MichaReiser on 2025-10-20 17:28_

---
