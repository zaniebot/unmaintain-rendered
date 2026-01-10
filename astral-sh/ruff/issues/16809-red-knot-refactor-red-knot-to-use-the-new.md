```yaml
number: 16809
title: "[red-knot] Refactor Red Knot to use the new `Diagnostic` type. Delete \"old\" diagnostic code."
type: issue
state: closed
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
created_at: 2025-03-17T15:07:19Z
updated_at: 2025-04-22T16:08:04Z
url: https://github.com/astral-sh/ruff/issues/16809
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] Refactor Red Knot to use the new `Diagnostic` type. Delete "old" diagnostic code.

---

_Issue opened by @BurntSushi on 2025-03-17 15:07_

With #16503, we added types defining a new `Diagnostic` data model. In #16711, we added a renderer based on that new data model. At present, Red Knot is still using the "old" diagnostic types. In this issue, we should move forward by using the new types in Red Knot.

I expect that this will _mostly_ be a mechanical translation that doesn't impact the diagnostic output too much, but it might be hard to resist doing some improvements along the way.

---

_Assigned to @BurntSushi by @BurntSushi on 2025-03-17 15:08_

---

_Label `red-knot` added by @BurntSushi on 2025-03-17 15:09_

---

_Label `diagnostics` added by @BurntSushi on 2025-03-17 15:09_

---

_Renamed from "Refactor Red Knot to use the new `Diagnostic` type. Delete "old" diagnostic code." to "[red-knot] Refactor Red Knot to use the new `Diagnostic` type. Delete "old" diagnostic code." by @BurntSushi on 2025-03-17 15:09_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:32_

---

_Removed from milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:33_

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:33_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Closed by @BurntSushi on 2025-04-22 16:08_

---
