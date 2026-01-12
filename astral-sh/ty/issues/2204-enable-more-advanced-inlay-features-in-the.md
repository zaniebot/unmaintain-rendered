```yaml
number: 2204
title: Enable more advanced inlay features in the playground
type: issue
state: closed
author: AlexWaygood
labels:
  - playground
assignees: []
created_at: 2025-12-24T12:48:52Z
updated_at: 2025-12-26T08:41:31Z
url: https://github.com/astral-sh/ty/issues/2204
synced_at: 2026-01-12T15:54:26Z
```

# Enable more advanced inlay features in the playground

---

_@AlexWaygood_

I've no idea if this is feasible, but it would be very cool if we could enable some of our more "advanced" inlay-hint features in the playground. For example, go-to definition on inlay hints doesn't currently work in the ty playground, and neither does "baking" of inlay hints.

---

_Label `server` added by @AlexWaygood on 2025-12-24 12:48_

---

_Label `playground` added by @AlexWaygood on 2025-12-24 12:48_

---

_Comment by @MichaReiser on 2025-12-24 13:14_

Interesting, we do set the edits on the inlay hints. 

But there's also this comment

https://github.com/astral-sh/ruff/blob/e5818d89fd435d63a526d0988876b6160f44f854/playground/ty/src/Editor/Editor.tsx#L459

---

_Comment by @MatthewMckee4 on 2025-12-26 00:43_

baking inlay hints seems to work at play.ty.dev. I will look into the goto more.

---

_Closed by @MichaReiser on 2025-12-26 08:41_

---

_Label `server` removed by @MichaReiser on 2025-12-26 08:41_

---
