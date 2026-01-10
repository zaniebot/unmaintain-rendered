---
number: 21332
title: "`unused-noqa` (`RUF100`) - `Unnecessary` diagnostic tags should only cover the unused rule"
type: issue
state: open
author: DetachHead
labels:
  - server
  - diagnostics
assignees: []
created_at: 2025-11-08T03:31:57Z
updated_at: 2025-11-26T07:10:22Z
url: https://github.com/astral-sh/ruff/issues/21332
synced_at: 2026-01-10T01:23:02Z
---

# `unused-noqa` (`RUF100`) - `Unnecessary` diagnostic tags should only cover the unused rule

---

_Issue opened by @DetachHead on 2025-11-08 03:31_

similar to #21331

<img width="727" height="50" alt="Image" src="https://github.com/user-attachments/assets/43de9889-fdd2-418b-adb3-f9b507d6073e" />

here, the greyed out comment makes it look like the entire `noqa` comment is unused, which isn't the case

---

_Renamed from "`unused-noqa` (`RUF100`) - `Unnecessary` diagnostic tags for should only cover the unused rule" to "`unused-noqa` (`RUF100`) - `Unnecessary` diagnostic tags should only cover the unused rule" by @DetachHead on 2025-11-08 03:32_

---

_Label `server` added by @ntBre on 2025-11-10 14:06_

---

_Label `diagnostics` added by @ntBre on 2025-11-10 14:06_

---

_Comment by @TaKO8Ki on 2025-11-12 02:08_

I added the tag to this rule, so I will take this issue.

---

_Comment by @MichaReiser on 2025-11-12 07:20_

Same here. I suspect that this is trickier to solve as it probably requires changes to `Diagnostic`

---

_Comment by @MichaReiser on 2025-11-26 07:10_

I think the solution here is to change the diagnostic range to only cover the code if the suppression contains multiple codes and only some codes are unused. Similar to what we do now in ty

---
