```yaml
number: 119
title: Avoid type inference diagnostics on nodes with invalid syntax
type: issue
state: open
author: ntBre
labels:
  - diagnostics
assignees: []
created_at: 2025-04-21T13:21:14Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/119
synced_at: 2026-01-12T15:54:22Z
```

# Avoid type inference diagnostics on nodes with invalid syntax

---

_@ntBre_

hmm, yeah, ideally we would get rid of the `invalid-type-form` error here; it's just redundant noise to the user to have two diagnostics for the same problem. I think @MichaReiser has mentioned in the past that he wanted a global solution to make sure we didn't emit any red-knot diagnostics from type inference on nodes that were already known to be invalid syntax. Would be another thing that would be good to file a followup issue for IMO.

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/17463#discussion_r2051164780_
            

---

_Renamed from "[red-knot] Avoid type inference diagnostics on nodes with invalid syntax" to "Avoid type inference diagnostics on nodes with invalid syntax" by @MichaReiser on 2025-05-07 15:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-11 07:39_

---

_Removed from milestone `Beta` by @carljm on 2025-07-23 23:23_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:33_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
