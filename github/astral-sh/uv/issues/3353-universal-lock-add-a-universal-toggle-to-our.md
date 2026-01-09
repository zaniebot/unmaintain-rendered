---
number: 3353
title: "universal-lock: add a \"universal\" toggle to our resolver"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T15:47:00Z
updated_at: 2024-05-09T13:24:38Z
url: https://github.com/astral-sh/uv/issues/3353
synced_at: 2026-01-07T13:12:17-06:00
---

# universal-lock: add a "universal" toggle to our resolver

---

_Issue opened by @BurntSushi on 2024-05-03 15:47_

As part of #3350, I think we will probably want to be able to toggle the resolver between "platform specific mode" and "universal mode." The latter requires some behavioral changes in how it does marker evaluation. It would also be useful to use this toggle to improve error handling in some cases. For example, in the former case, resolver forking should never be allowed. So if the universal toggle is disabled, then we shouldn't try to turn an "unresolvable" case into a resolvable case by forking.

---

_Label `preview` added by @BurntSushi on 2024-05-03 15:47_

---

_Referenced in [astral-sh/uv#3350](../../astral-sh/uv/issues/3350.md) on 2024-05-03 15:47_

---

_Referenced in [astral-sh/uv#3466](../../astral-sh/uv/pulls/3466.md) on 2024-05-08 18:10_

---

_Closed by @BurntSushi on 2024-05-09 13:24_

---
