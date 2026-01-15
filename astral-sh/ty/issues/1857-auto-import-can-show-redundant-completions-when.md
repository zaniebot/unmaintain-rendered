```yaml
number: 1857
title: Auto-import can show redundant completions when the same item is exposed from multiple modules
type: issue
state: closed
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-11T18:03:36Z
updated_at: 2026-01-15T17:31:08Z
url: https://github.com/astral-sh/ty/issues/1857
synced_at: 2026-01-15T17:50:06Z
```

# Auto-import can show redundant completions when the same item is exposed from multiple modules

---

_@BurntSushi_

A good example of this is `read_csv` from Pandas. The first result is the one we want:

https://github.com/user-attachments/assets/2ae60909-ff2c-4279-b371-c2c07593eff5

But it also shows `read_csv` from a bunch of other sub-modules. Indeed, these are all the same symbol. They all share the same definition in `pandas/io/parsers/readers.py`.

I think we should show only _one_ of them, and probably the one from the top-most module. This seems to be what pylance does:

https://github.com/user-attachments/assets/ce075b94-3db8-44a6-84fd-6b7b420fdc88

---

_Added to milestone `Stable` by @BurntSushi on 2025-12-11 18:03_

---

_Label `server` added by @BurntSushi on 2025-12-11 18:03_

---

_Label `completions` added by @BurntSushi on 2025-12-11 18:03_

---

_Comment by @BurntSushi on 2025-12-11 18:44_

A somewhat related suggestion from @AlexWaygood here: https://github.com/astral-sh/ty/issues/1274#issuecomment-3517028220

---

_Comment by @BurntSushi on 2025-12-17 14:17_

A related thing here is ranking sub-modules named `foo` higher than top-level modules named `foo`: https://github.com/astral-sh/ty/issues/1274#issuecomment-3665291440

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:35_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:35_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 03:49_

---

_Unassigned @Gankra by @BurntSushi on 2026-01-09 15:06_

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-09 15:06_

---

_Closed by @BurntSushi on 2026-01-15 17:31_

---
