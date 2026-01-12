```yaml
number: 126
title: model the possibility of mid-module external mutation of module globals?
type: issue
state: open
author: carljm
labels:
  - runtime semantics
assignees: []
created_at: 2025-04-14T23:03:04Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/126
synced_at: 2026-01-12T15:54:22Z
```

# model the possibility of mid-module external mutation of module globals?

---

_@carljm_

It is possible for module-level code to execute some code (via call and/or import) which in turn mutates the currently-executing module's globals.

Currently we don't respect this possibility.

If we did, it would mean that all module-level code would need to rely only on declared types of module-level variables, never a locally-inferred type.

---

_Renamed from "[red-knot] model the possibility of mid-module external mutation of module globals?" to "model the possibility of mid-module external mutation of module globals?" by @MichaReiser on 2025-05-07 15:25_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:38_

---

_Removed from milestone `Beta` by @carljm on 2025-07-23 23:22_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:31_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
