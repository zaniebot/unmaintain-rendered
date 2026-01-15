```yaml
number: 1598
title: "Consolidate `type[]` types in a union when displaying them"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-11-20T11:48:53Z
updated_at: 2026-01-15T15:21:01Z
url: https://github.com/astral-sh/ty/issues/1598
synced_at: 2026-01-15T15:50:04Z
```

# Consolidate `type[]` types in a union when displaying them

---

_@AlexWaygood_

When displaying a union (e.g. in error messages or on hover), it would be nicer if we displayed the union `type[A] | type[B] | type[C]` as `type[A | B | C]`. The latter means the same thing, is a valid type annotation, and is much more concise. This would be similar to the way we display `Literal["a"] | Literal[42] | Literal[b"foooo"]` as `Literal["a", 42, b"foo"]` in diagnostics and on hover.

Keeping type display concise where possible makes our diagnostics much more readable.

---

_Label `help wanted` added by @AlexWaygood on 2025-11-20 11:48_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-20 11:48_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-20 12:20_

---

_Renamed from "Consolidate `type[]` types in a union" to "Consolidate `type[]` types in a union when displaying them" by @AlexWaygood on 2025-11-20 14:07_

---

_Comment by @mahiro72 on 2025-12-18 14:23_

Hi @AlexWaygood , I'd like to work on this issue.

---

_Comment by @AlexWaygood on 2025-12-18 14:29_

@mahiro72 go for it!

---

_Assigned to @mahiro72 by @AlexWaygood on 2025-12-18 14:29_

---

_Comment by @AlexWaygood on 2025-12-18 14:30_

Please ping us if you need a hand with anything :-)

---

_Comment by @11happy on 2026-01-13 17:02_

Hello @mahiro72 are you still working on this ? If not may I work on this ?
Thank you

---

_Comment by @AlexWaygood on 2026-01-13 17:19_

It's been a month -- @11happy feel free to take a look :-)

---

_Unassigned @mahiro72 by @AlexWaygood on 2026-01-13 17:19_

---

_Closed by @AlexWaygood on 2026-01-15 15:21_

---
