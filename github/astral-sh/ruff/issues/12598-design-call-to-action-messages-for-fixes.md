---
number: 12598
title: "Design call to action messages for fixes displayed in `check`"
type: issue
state: open
author: chriskrycho
labels:
  - fixes
  - cli
assignees: []
created_at: 2024-07-31T18:05:05Z
updated_at: 2024-07-31T21:41:54Z
url: https://github.com/astral-sh/ruff/issues/12598
synced_at: 2026-01-07T13:12:15-06:00
---

# Design call to action messages for fixes displayed in `check`

---

_Issue opened by @chriskrycho on 2024-07-31 18:05_

While working on #12595, @zanieb suggested to split out the design for calls-to-action for unsafe fixes and display-only fixes, since it is not obvious what the design there should be. A few key points to consider:

- Should there be per-fix calls-to-action (which is what we started out with) or only a summary?
- Should unsafe fixes be displayed by default? As of the status quo at the end of #12595, the answer is “no”, but maybe they should in `check` mode, or perhaps there should be a flag to display *all* fixes. The new behavior means that there is actually no way *to* display those fixes without also running the fixer.
- What should the language and formatting be? Color? Bold? etc.?


---

_Label `fixes` added by @AlexWaygood on 2024-07-31 18:16_

---

_Renamed from "Design call to action messages for unsafe and display-only fixes displayed in `check`" to "Design call to action messages for fixes displayed in `check`" by @chriskrycho on 2024-07-31 18:20_

---

_Referenced in [astral-sh/ruff#12595](../../astral-sh/ruff/pulls/12595.md) on 2024-07-31 18:21_

---

_Label `cli` added by @zanieb on 2024-07-31 21:41_

---

_Referenced in [astral-sh/ruff#19919](../../astral-sh/ruff/pulls/19919.md) on 2025-08-14 19:41_

---
