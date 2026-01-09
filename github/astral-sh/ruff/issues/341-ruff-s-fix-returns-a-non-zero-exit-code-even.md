---
number: 341
title: "ruff's `--fix` returns a non-zero exit code even though there's only one fixable lint error."
type: issue
state: closed
author: snapdgn
labels: []
assignees: []
created_at: 2022-10-07T16:08:16Z
updated_at: 2022-10-07T16:19:47Z
url: https://github.com/astral-sh/ruff/issues/341
synced_at: 2026-01-07T13:12:14-06:00
---

# ruff's `--fix` returns a non-zero exit code even though there's only one fixable lint error.

---

_Issue opened by @snapdgn on 2022-10-07 16:08_

code snippet to reproduce the issue:
```python
import json

def fn():
    pass
```
command: `ruff . --fix`

output: ```No pyproject.toml found.
Falling back to default configuration...
Found 0 error(s) (1 fixed).```

`echo $?` gives 1(as exit code)

Image for reference:
![image](https://user-images.githubusercontent.com/85608760/194597790-c444218f-20a1-4477-897f-4c948890144a.png)


---

_Comment by @charliermarsh on 2022-10-07 16:09_

Oh thanks! Will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-07 16:09_

---

_Referenced in [cnpryer/huak#282](../../cnpryer/huak/pulls/282.md) on 2022-10-07 16:12_

---

_Referenced in [astral-sh/ruff#342](../../astral-sh/ruff/pulls/342.md) on 2022-10-07 16:13_

---

_Closed by @charliermarsh on 2022-10-07 16:13_

---

_Comment by @charliermarsh on 2022-10-07 16:14_

[v0.0.58](https://github.com/charliermarsh/ruff/releases/tag/v0.0.58) is building now.

---

_Renamed from "ruff's `--fix` return a non-zero exit code even though there's only one fixable lint error." to "ruff's `--fix` returns a non-zero exit code even though there's only one fixable lint error." by @snapdgn on 2022-10-07 16:19_

---

_Referenced in [astral-sh/ruff#2633](../../astral-sh/ruff/issues/2633.md) on 2023-02-07 19:31_

---

_Referenced in [codegrande/ruff#4](../../codegrande/ruff/pulls/4.md) on 2024-05-17 07:07_

---
