---
number: 11536
title: "Add documentation for Python version requests in `uv tool install`"
type: issue
state: closed
author: nick-voisin
labels:
  - documentation
assignees: []
created_at: 2025-02-15T14:27:58Z
updated_at: 2025-02-18T17:45:59Z
url: https://github.com/astral-sh/uv/issues/11536
synced_at: 2026-01-10T01:25:07Z
---

# Add documentation for Python version requests in `uv tool install`

---

_Issue opened by @nick-voisin on 2025-02-15 14:27_

### Summary

By default when installing a tool, uv uses the latest and first python binary it finds.
It would be great we could choose in which python version, to install a specific tool.

### Example

`uv tool install black --python=3.10`

---

_Label `enhancement` added by @nick-voisin on 2025-02-15 14:27_

---

_Comment by @FishAlchemist on 2025-02-15 15:53_

This feature already exists. Which version of UV are you using?
![Image](https://github.com/user-attachments/assets/6ef2231f-5434-4816-93f7-89d11b62ad6a)

---

_Comment by @zanieb on 2025-02-15 15:54_

Yeah this is supported exactly as written.

---

_Closed by @zanieb on 2025-02-15 15:54_

---

_Comment by @nick-voisin on 2025-02-15 17:50_

I did not see it in the [tools](https://docs.astral.sh/uv/guides/tools/) docs, so I thought it wasn't available.
It is however present in the CLI via `uv help tool install`, but its buried _very_ deep.

Using 0.5.30, just updated to 0.60

My bad! thanks 

---

_Comment by @charliermarsh on 2025-02-15 19:15_

Thanks for following up!

---

_Reopened by @zanieb on 2025-02-15 20:09_

---

_Label `enhancement` removed by @zanieb on 2025-02-15 20:09_

---

_Label `documentation` added by @zanieb on 2025-02-15 20:09_

---

_Renamed from "Enable python version selection using uv tool install" to "Add documentation for Python version requests in `uv tool install`" by @zanieb on 2025-02-15 20:09_

---

_Comment by @zanieb on 2025-02-15 20:09_

We can add that, good catch!

---

_Referenced in [astral-sh/uv#11598](../../astral-sh/uv/pulls/11598.md) on 2025-02-18 13:50_

---

_Closed by @zanieb on 2025-02-18 17:45_

---
