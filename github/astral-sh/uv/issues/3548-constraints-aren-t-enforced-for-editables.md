---
number: 3548
title: "Constraints aren't enforced for editables"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-05-13T14:44:08Z
updated_at: 2024-05-13T17:06:28Z
url: https://github.com/astral-sh/uv/issues/3548
synced_at: 2026-01-07T13:12:17-06:00
---

# Constraints aren't enforced for editables

---

_Issue opened by @charliermarsh on 2024-05-13 14:44_

If you run `uv pip install -e ./scripts/packages/black_editable --constraint constraints.txt` where `constraints.txt` contains `black==0.0.1`, the command succeeds.

Meanwhile, `uv pip install ./scripts/packages/black_editable --constraint constraints.txt` fails (correctly) with:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of black==0.0.1 and you require black==0.0.1, we can conclude that the requirements are unsatisfiable
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-13 14:44_

---

_Label `bug` added by @charliermarsh on 2024-05-13 14:44_

---

_Referenced in [astral-sh/uv#3554](../../astral-sh/uv/pulls/3554.md) on 2024-05-13 16:52_

---

_Closed by @charliermarsh on 2024-05-13 17:06_

---
