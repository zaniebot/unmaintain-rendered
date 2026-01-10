---
number: 7328
title: "Support `uv tree --script my_script.py`"
type: issue
state: closed
author: aivarannamaa
labels:
  - enhancement
  - good first issue
  - help wanted
assignees: []
created_at: 2024-09-12T11:50:22Z
updated_at: 2025-01-08T21:32:48Z
url: https://github.com/astral-sh/uv/issues/7328
synced_at: 2026-01-10T01:24:13Z
---

# Support `uv tree --script my_script.py`

---

_Issue opened by @aivarannamaa on 2024-09-12 11:50_

It would be nice if `uv tree` could extract information about dependencies also from scripts with inline metadata.

---

_Label `enhancement` added by @charliermarsh on 2024-09-13 13:45_

---

_Comment by @charliermarsh on 2024-09-13 13:45_

Seems reasonable although `uv tree` operates on a lockfile which scripts don't have, so we'd have to do a resolve each time.

---

_Referenced in [astral-sh/uv#5613](../../astral-sh/uv/issues/5613.md) on 2024-09-13 23:31_

---

_Label `help wanted` added by @charliermarsh on 2024-10-28 00:02_

---

_Label `good first issue` added by @charliermarsh on 2024-10-28 00:02_

---

_Comment by @mahmudsudo on 2024-11-04 20:11_

can i take on this issue ?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-25 19:02_

---

_Referenced in [astral-sh/uv#10159](../../astral-sh/uv/pulls/10159.md) on 2024-12-25 19:02_

---

_Closed by @charliermarsh on 2025-01-08 21:32_

---

_Closed by @charliermarsh on 2025-01-08 21:32_

---
