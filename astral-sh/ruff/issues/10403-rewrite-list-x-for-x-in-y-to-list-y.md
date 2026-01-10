---
number: 10403
title: "Rewrite `list(x for x in y)` to `list(y)`"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-03-14T02:38:53Z
updated_at: 2024-03-15T14:33:27Z
url: https://github.com/astral-sh/ruff/issues/10403
synced_at: 2026-01-10T01:22:50Z
---

# Rewrite `list(x for x in y)` to `list(y)`

---

_Issue opened by @charliermarsh on 2024-03-14 02:38_

Given `list(x for x in y)`, we first hit C400, which fixes to `[x for x in y]`, which hits C416, which fixes to `list(y)`. We should consider short-circuiting this.

---

_Label `rule` added by @charliermarsh on 2024-03-14 02:38_

---

_Label `help wanted` added by @charliermarsh on 2024-03-14 02:38_

---

_Comment by @hikaru-kajita on 2024-03-15 03:39_

I would like to contribute on this. Should I implement it on inside C400 or add an another rule which matches only expression of type `list(x for x in y)`?

---

_Comment by @charliermarsh on 2024-03-15 03:43_

I think we should augment C400 to support it.

---

_Referenced in [astral-sh/ruff#10419](../../astral-sh/ruff/pulls/10419.md) on 2024-03-15 08:11_

---

_Comment by @charliermarsh on 2024-03-15 14:33_

Closed by https://github.com/astral-sh/ruff/pull/10419. Thanks @boolean-light!

---

_Closed by @charliermarsh on 2024-03-15 14:33_

---
