---
number: 11382
title: "`ruff server`: Code action for re-enabling rules when cursor is on noqa comments"
type: issue
state: open
author: T-256
labels:
  - server
assignees: []
created_at: 2024-05-12T22:06:18Z
updated_at: 2025-04-03T13:48:09Z
url: https://github.com/astral-sh/ruff/issues/11382
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server`: Code action for re-enabling rules when cursor is on noqa comments

---

_Issue opened by @T-256 on 2024-05-12 22:06_

Could it possible instead of clearing noqa by hand, add code action when moving cursor on noqa code comments?
For example, moving cursor on `# noqa:`, show these code actions:
- Ruff (ANN201): Re-enable for this line.
- Ruff (D103): Re-enable for this line.
- Re-enable all disabled rules for this line.

_Originally posted by @T-256 in https://github.com/astral-sh/ruff/issues/11276#issuecomment-2094128767_
            

---

_Label `server` added by @AlexWaygood on 2024-05-12 22:24_

---

_Comment by @T-256 on 2024-05-24 21:26_

Closing as it's not possible with today LSP specification to determine the cursor position.

---

_Closed by @T-256 on 2024-05-24 21:26_

---

_Comment by @MichaReiser on 2024-05-27 11:18_

Would it be possible to implement this as a potential refactor? I'm not that familiar with the LSP spec but there must be a way to publish code actions based on the current position (e.g. refactors)

---

_Comment by @MichaReiser on 2024-05-27 11:21_

Actually, this is supported by the code action request. It sends an optional `range` field that can be used to provide additional code actions.

---

_Reopened by @T-256 on 2024-05-27 11:59_

---

_Comment by @snowsignal on 2024-06-03 20:20_

I think we could do this! (And it would be a nice feature to have)

---

_Assigned to @snowsignal by @snowsignal on 2024-06-03 20:21_

---

_Added to milestone `Ruff Server: Post-Stable` by @snowsignal on 2024-06-17 16:33_

---

_Removed from milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2025-04-03 13:48_

---
