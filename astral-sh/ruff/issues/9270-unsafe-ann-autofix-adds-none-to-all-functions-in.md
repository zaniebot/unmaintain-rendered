---
number: 9270
title: "unsafe ANN autofix adds -> None to all functions in pyi files"
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-12-25T03:17:08Z
updated_at: 2023-12-25T14:03:25Z
url: https://github.com/astral-sh/ruff/issues/9270
synced_at: 2026-01-10T01:22:49Z
---

# unsafe ANN autofix adds -> None to all functions in pyi files

---

_Issue opened by @twoertwein on 2023-12-25 03:17_

`ruff --select "ANN001,ANN2" --fix-only` (version 0.1.9) adds `-> None` to all functions in pyi files as they have no implementation in the stub files.

xref https://github.com/astral-sh/ruff/issues/9004#issuecomment-1839976078

---

_Comment by @charliermarsh on 2023-12-25 13:11_

Thanks, will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-25 13:43_

---

_Label `bug` added by @charliermarsh on 2023-12-25 13:43_

---

_Referenced in [astral-sh/ruff#9277](../../astral-sh/ruff/pulls/9277.md) on 2023-12-25 13:47_

---

_Closed by @charliermarsh on 2023-12-25 14:03_

---
