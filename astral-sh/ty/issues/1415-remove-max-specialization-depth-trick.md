```yaml
number: 1415
title: Remove max-specialization-depth trick
type: issue
state: closed
author: sharkdp
labels: []
assignees: []
created_at: 2025-10-23T09:00:44Z
updated_at: 2025-11-26T16:50:28Z
url: https://github.com/astral-sh/ty/issues/1415
synced_at: 2026-01-12T15:54:25Z
```

# Remove max-specialization-depth trick

---

_@sharkdp_

Revert the changes in https://github.com/astral-sh/ruff/pull/20988 and only fall back to `C[Divergent]` is there are actual cyclic references, not if there are simply too many layers of specialization.

In other words, prevent the `Divergent` type in an example like this: https://play.ty.dev/e9b94a3d-704c-4078-90fc-ed9915cfb5e7

---

_Comment by @MichaReiser on 2025-10-23 09:06_

Relevant PR, https://github.com/astral-sh/ruff/pull/20566

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-23 13:36_

---

_Closed by @carljm on 2025-11-26 16:50_

---
