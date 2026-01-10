---
number: 249
title: "`venv` creation fails when recreating a `venv` from within that `venv`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-10-31T03:43:54Z
updated_at: 2023-10-31T12:22:53Z
url: https://github.com/astral-sh/uv/issues/249
synced_at: 2026-01-10T01:23:03Z
---

# `venv` creation fails when recreating a `venv` from within that `venv`

---

_Issue opened by @charliermarsh on 2023-10-31 03:43_

If I run `cargo run -p puffin-cli -- venv .venv`, then `source .venv/bin/activate`, then `cargo run -p puffin-cli -- venv .venv`, I end up with a busted `venv`, because the second creation uses the interpreter from the first creation, which is then deleted.

---

_Label `bug` added by @charliermarsh on 2023-10-31 03:43_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-31 03:44_

---

_Removed from milestone `Initial release` by @charliermarsh on 2023-10-31 03:44_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-31 03:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-31 03:46_

---

_Referenced in [astral-sh/uv#250](../../astral-sh/uv/pulls/250.md) on 2023-10-31 03:47_

---

_Closed by @charliermarsh on 2023-10-31 12:22_

---
