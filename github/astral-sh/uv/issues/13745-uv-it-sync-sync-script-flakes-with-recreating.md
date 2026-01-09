---
number: 13745
title: "`uv::it sync::sync_script` flakes with \"Recreating script environment\""
type: issue
state: closed
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-05-30T21:14:48Z
updated_at: 2025-06-30T15:39:49Z
url: https://github.com/astral-sh/uv/issues/13745
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv::it sync::sync_script` flakes with "Recreating script environment"

---

_Issue opened by @zanieb on 2025-05-30 21:14_

See https://github.com/astral-sh/uv/issues/13744#issuecomment-2923510476

Are these failures always occurring together?

---

_Label `ci-flake` added by @zanieb on 2025-05-30 21:16_

---

_Comment by @konstin on 2025-06-02 07:46_

I wonder if there's another test touching the underlying Pythons? An approach at debugging this is adding the reason why the venv would be recreated to the message. We have the reason internally, but we don't pass it on until the message we show.

---

_Referenced in [astral-sh/uv#13748](../../astral-sh/uv/pulls/13748.md) on 2025-06-02 17:04_

---

_Comment by @Gankra on 2025-06-11 13:45_

Another instance https://github.com/astral-sh/uv/actions/runs/15586266130/job/43893192186?pr=13965

---

_Referenced in [astral-sh/uv#13965](../../astral-sh/uv/pulls/13965.md) on 2025-06-11 13:45_

---

_Referenced in [astral-sh/uv#14160](../../astral-sh/uv/issues/14160.md) on 2025-06-20 15:36_

---

_Referenced in [astral-sh/uv#14331](../../astral-sh/uv/pulls/14331.md) on 2025-06-27 20:35_

---

_Closed by @zanieb on 2025-06-30 15:39_

---
