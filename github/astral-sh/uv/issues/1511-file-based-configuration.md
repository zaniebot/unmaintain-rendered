---
number: 1511
title: File-based configuration
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-02-16T16:29:38Z
updated_at: 2024-05-01T16:34:25Z
url: https://github.com/astral-sh/uv/issues/1511
synced_at: 2026-01-07T13:12:16-06:00
---

# File-based configuration

---

_Issue opened by @zanieb on 2024-02-16 16:29_

We should support configuration via a file, but what this looks like exactly is not yet determined.



---

_Label `enhancement` added by @zanieb on 2024-02-16 16:29_

---

_Label `configuration` added by @zanieb on 2024-02-16 16:29_

---

_Referenced in [astral-sh/uv#1509](../../astral-sh/uv/pulls/1509.md) on 2024-02-16 16:30_

---

_Comment by @DanCardin on 2024-02-16 16:39_

Please dont use `dirs`, as they've explicitly rejected the idea of supporting XDG on macos, which is really annoying to the people that care, and irrelevant to the people that dont care. You'll eventually (or immediately) get requests to do so, and will have to work around their weird stance. Nushell has had a many years long [open issue](https://github.com/nushell/nushell/issues/893#issuecomment-1872575029) as a result of their early choice to use `app_dirs`.

I've had good success with `etcetera`, although I'm certain there are other options for arriving at mutually liked behavior.

---

_Referenced in [astral-sh/ruff#10739](../../astral-sh/ruff/issues/10739.md) on 2024-04-02 14:39_

---

_Referenced in [astral-sh/uv#3019](../../astral-sh/uv/issues/3019.md) on 2024-04-15 20:31_

---

_Referenced in [astral-sh/uv#3248](../../astral-sh/uv/issues/3248.md) on 2024-04-24 17:09_

---

_Closed by @charliermarsh on 2024-05-01 16:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-01 16:34_

---
