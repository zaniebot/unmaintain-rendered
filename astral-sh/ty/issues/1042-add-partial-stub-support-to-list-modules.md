```yaml
number: 1042
title: "add partial stub support to \"list modules\" implementation"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-08-18T12:45:15Z
updated_at: 2026-01-09T15:32:57Z
url: https://github.com/astral-sh/ty/issues/1042
synced_at: 2026-01-12T15:54:24Z
```

# add partial stub support to "list modules" implementation

---

_@BurntSushi_

Once https://github.com/astral-sh/ruff/pull/19931 is merged, we should add similar logic to `list_modules` (which is itself still unmerged at time of writing in https://github.com/astral-sh/ruff/pull/19883).

Ideally this would also update the module resolution flow diagram as well.

Currently this only impacts completions as completions are the only thing that uses `list_modules`.

---

_Added to milestone `GA` by @BurntSushi on 2025-08-18 12:45_

---

_Label `server` added by @BurntSushi on 2025-08-18 12:45_

---

_Assigned to @Gankra by @Gankra on 2025-08-20 13:15_

---

_Comment by @Gankra on 2025-08-20 13:15_

Tentatively assigning myself so it doesn't slip through the cracks, but fine if someone else grabs this

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:49_

---

_Comment by @MichaReiser on 2025-11-14 08:47_

@Gankra are you still planning to work on this?

---

_Comment by @Gankra on 2025-11-14 14:50_

Yes.

---

_Removed from milestone `Stable` by @Gankra on 2026-01-09 15:32_

---

_Added to milestone `Pre-stable 1` by @Gankra on 2026-01-09 15:32_

---
