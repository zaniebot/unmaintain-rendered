```yaml
number: 246
title: consider building use-def map per-scope instead of per-module
type: issue
state: open
author: carljm
labels:
  - performance
assignees: []
created_at: 2024-05-31T22:03:02Z
updated_at: 2025-05-07T15:27:43Z
url: https://github.com/astral-sh/ty/issues/246
synced_at: 2026-01-10T02:34:09Z
```

# consider building use-def map per-scope instead of per-module

---

_Issue opened by @carljm on 2024-05-31 22:03_

Currently we always build symbol tables and definitions for an entire module at once.

We could consider deferring building these for functions until we actually need to check them.


---

_Renamed from "[red-knot] consider building symbol tables / definitions / CFG lazily" to "[red-knot] consider building symbol tables / definitions / CFG lazily within a module" by @carljm on 2024-05-31 22:20_

---

_Label `performance` added by @carljm on 2024-08-05 22:10_

---

_Renamed from "[red-knot] consider building symbol tables / definitions / CFG lazily within a module" to "[red-knot] consider building use-def map per-scope instead of per-module" by @carljm on 2024-08-24 02:20_

---

_Renamed from "[red-knot] consider building use-def map per-scope instead of per-module" to "consider building use-def map per-scope instead of per-module" by @MichaReiser on 2025-05-07 15:27_

---
