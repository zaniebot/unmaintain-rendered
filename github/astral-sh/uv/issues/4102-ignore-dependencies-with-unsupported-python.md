---
number: 4102
title: Ignore dependencies with unsupported Python versions during universal lock
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-06T14:44:23Z
updated_at: 2024-06-26T12:55:13Z
url: https://github.com/astral-sh/uv/issues/4102
synced_at: 2026-01-07T13:12:17-06:00
---

# Ignore dependencies with unsupported Python versions during universal lock

---

_Issue opened by @charliermarsh on 2024-06-06 14:44_

If the user has `requires-python = ">= 3.7"`, we should be able to prune dependencies with (e.g.) `flask ; python_version == '3.3'`.


---

_Label `bug` added by @charliermarsh on 2024-06-06 14:44_

---

_Label `preview` added by @charliermarsh on 2024-06-06 14:44_

---

_Referenced in [astral-sh/uv#4099](../../astral-sh/uv/issues/4099.md) on 2024-06-06 14:44_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-06-06 14:44_

---

_Referenced in [astral-sh/uv#4194](../../astral-sh/uv/issues/4194.md) on 2024-06-10 12:49_

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-06-10 13:07_

---

_Comment by @BurntSushi on 2024-06-26 12:55_

Closed by https://github.com/astral-sh/uv/pull/4273

---

_Closed by @BurntSushi on 2024-06-26 12:55_

---
