---
number: 21331
title: "`deprecated-import` (`UP035`) - `Deprecated` diagnostic tags should only span the symbol that's actually deprecated"
type: issue
state: open
author: DetachHead
labels:
  - server
  - diagnostics
assignees: []
created_at: 2025-11-08T03:30:17Z
updated_at: 2025-11-12T07:19:31Z
url: https://github.com/astral-sh/ruff/issues/21331
synced_at: 2026-01-07T13:12:16-06:00
---

# `deprecated-import` (`UP035`) - `Deprecated` diagnostic tags should only span the symbol that's actually deprecated

---

_Issue opened by @DetachHead on 2025-11-08 03:30_

for example:

<img width="636" height="106" alt="Image" src="https://github.com/user-attachments/assets/75d86873-5687-4f97-be4a-ffc4e9310d52" />

this makes it look like the whole `typing_extensions` module is deprecated or something, which isn't the case.

other language servers that use the `Deprecated` tag will only use it on the deprecated symbol itself, for example pyright:

<img width="663" height="45" alt="Image" src="https://github.com/user-attachments/assets/91e6bb8c-3abd-4e1b-ba60-afecd295b322" />

i think the fact that the diagnostic covers the whole import statement isn't much of an issue when diagnostic tags are not used, but when they are, it can be a bit misleading.

---

_Referenced in [astral-sh/ruff#21332](../../astral-sh/ruff/issues/21332.md) on 2025-11-08 03:31_

---

_Renamed from "`Deprecated` diagnostic tags should only span the symbol that's actually deprecated" to "`deprecated-import` (`UP035`) - `Deprecated` diagnostic tags should only span the symbol that's actually deprecated" by @DetachHead on 2025-11-08 03:36_

---

_Label `server` added by @MichaReiser on 2025-11-08 17:22_

---

_Label `diagnostics` added by @MichaReiser on 2025-11-08 17:22_

---

_Comment by @TaKO8Ki on 2025-11-12 02:08_

I will take this issue.

---

_Comment by @MichaReiser on 2025-11-12 07:19_

This might be trickier to solve as it probably requires changes to `Diagnostic`

---
