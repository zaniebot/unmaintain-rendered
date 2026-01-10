---
number: 3409
title: "Preserve url parsing in `Dist`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-05-06T14:16:51Z
updated_at: 2024-05-22T13:26:11Z
url: https://github.com/astral-sh/uv/issues/3409
synced_at: 2026-01-10T01:23:27Z
---

# Preserve url parsing in `Dist`

---

_Issue opened by @konstin on 2024-05-06 14:16_

Part of #3407. We first need to
* Decide which types to add to the `Dist` variants
* Add these types
* Get this information from the`PubGrubPackage`
* Use it in subsequent code over the `VerbatimUrl`

---

_Label `enhancement` added by @konstin on 2024-05-06 14:16_

---

_Assigned to @konstin by @konstin on 2024-05-06 14:16_

---

_Referenced in [astral-sh/uv#3407](../../astral-sh/uv/issues/3407.md) on 2024-05-06 14:21_

---

_Referenced in [astral-sh/uv#3429](../../astral-sh/uv/pulls/3429.md) on 2024-05-08 13:42_

---

_Comment by @charliermarsh on 2024-05-22 13:20_

Is this specifically for `Dist::from_url`?

---

_Comment by @konstin on 2024-05-22 13:24_

This is done, we track all the fields we need now

---

_Closed by @konstin on 2024-05-22 13:24_

---

_Comment by @charliermarsh on 2024-05-22 13:26_

Oh nice.

---
