---
number: 2706
title: Do not suggest PTH115 for Python < 3.9
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-02-10T03:11:14Z
updated_at: 2023-02-10T03:57:33Z
url: https://github.com/astral-sh/ruff/issues/2706
synced_at: 2026-01-07T13:12:14-06:00
---

# Do not suggest PTH115 for Python < 3.9

---

_Issue opened by @adamtheturtle on 2023-02-10 03:11_

Thank you for this great project.

`PTH115` suggests:

> os.readlink should be replaced by .readlink()

`Path.readlink()` was introduced in Python 3.9, so this should only be shown for Python >= 3.9.

---

_Comment by @charliermarsh on 2023-02-10 03:11_

Ah thank you, will fix.

---

_Label `bug` added by @charliermarsh on 2023-02-10 03:11_

---

_Label `good first issue` added by @charliermarsh on 2023-02-10 03:11_

---

_Referenced in [astral-sh/ruff#2708](../../astral-sh/ruff/pulls/2708.md) on 2023-02-10 03:33_

---

_Closed by @charliermarsh on 2023-02-10 03:57_

---
