```yaml
number: 238
title: support del statement and deletion of except handler names
type: issue
state: closed
author: carljm
labels:
  - runtime semantics
assignees: []
created_at: 2024-10-05T14:41:17Z
updated_at: 2025-06-12T14:44:43Z
url: https://github.com/astral-sh/ty/issues/238
synced_at: 2026-01-12T15:54:22Z
```

# support del statement and deletion of except handler names

---

_@carljm_

At runtime, names bound in an except handler are `del` ed at the end of the except handler block. We should model this as if there were a `del` statement for the name at the end of the block. And we should also support real `del` statements.

In both cases, it should take the form of a `Definition` that causes the name to be unbound.

---

_Closed by @AlexWaygood on 2024-10-05 16:59_

---

_Reopened by @AlexWaygood on 2024-10-05 17:03_

---

_Comment by @AlexWaygood on 2024-10-05 17:03_

Oops, accidentally closed this. It's not fixed!

---

_Renamed from "[red-knot] support del statement and deletion of except handler names" to "support del statement and deletion of except handler names" by @MichaReiser on 2025-05-07 15:27_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:18_

---

_Closed by @carljm on 2025-06-12 14:44_

---
