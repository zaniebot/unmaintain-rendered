---
number: 2814
title: "Upgrade `reqwest`"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-04-03T17:53:26Z
updated_at: 2024-04-10T15:20:46Z
url: https://github.com/astral-sh/uv/issues/2814
synced_at: 2026-01-10T01:23:22Z
---

# Upgrade `reqwest`

---

_Issue opened by @charliermarsh on 2024-04-03 17:53_

We should be able to do this shortly. I can handle it since it will require some changes to the cert logic.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-03 17:53_

---

_Label `internal` added by @charliermarsh on 2024-04-03 17:53_

---

_Comment by @samypr100 on 2024-04-04 01:20_

If you also might need a reference to adjust hyper usage in tests to 1.x, 35eee6d53bdaecea9777635ac74421af42bf9012 commit had my migration from 1.x to 0.14.x back from https://github.com/astral-sh/uv/pull/2136, so the reverse is what'd you need. Happy to help as well.


---

_Referenced in [astral-sh/uv#2817](../../astral-sh/uv/pulls/2817.md) on 2024-04-04 01:54_

---

_Referenced in [astral-sh/uv#2885](../../astral-sh/uv/pulls/2885.md) on 2024-04-08 03:57_

---

_Referenced in [astral-sh/uv#2884](../../astral-sh/uv/pulls/2884.md) on 2024-04-08 03:58_

---

_Referenced in [astral-sh/uv#2894](../../astral-sh/uv/pulls/2894.md) on 2024-04-08 04:31_

---

_Referenced in [astral-sh/uv#2895](../../astral-sh/uv/pulls/2895.md) on 2024-04-08 04:31_

---

_Referenced in [astral-sh/uv#2892](../../astral-sh/uv/pulls/2892.md) on 2024-04-08 04:31_

---

_Closed by @charliermarsh on 2024-04-10 15:20_

---
