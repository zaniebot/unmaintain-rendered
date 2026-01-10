---
number: 1367
title: "Within-revision LRU collection for queries returning values with lifetime `&'static`"
type: issue
state: open
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-10-16T11:33:15Z
updated_at: 2025-12-31T15:33:52Z
url: https://github.com/astral-sh/ty/issues/1367
synced_at: 2026-01-10T01:51:14Z
---

# Within-revision LRU collection for queries returning values with lifetime `&'static`

---

_Issue opened by @MichaReiser on 2025-10-16 11:33_

Salsa's fixpoint iteration is a very convenient feature to handle recursive queries as seen in https://github.com/astral-sh/ruff/pull/20477. The caching of some of the type operations can also help with performance. 

However, queries like `is_redundant_with` come at a high memory cost because there are simply so many instances that need to be cached. 

We should explore adding within-revision LRU garbage collection to Salsa for queries that return values with lifetime `&'static` (and don't return references). That is, values that are copied or cloned and, thus, collecting them in salsa can't lead to dangling references. 



---

_Label `memory` added by @MichaReiser on 2025-10-16 11:33_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:33_

---

_Comment by @MichaReiser on 2025-12-31 15:33_

The priority here will depend on user feedback (are they running into memory issues)

---
