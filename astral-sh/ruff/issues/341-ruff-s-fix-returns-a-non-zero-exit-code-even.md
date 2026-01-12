```yaml
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
synced_at: 2026-01-12T15:54:40Z
```

# ruff's `--fix` returns a non-zero exit code even though there's only one fixable lint error.

---

_@snapdgn_

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

_Closed by @charliermarsh on 2022-10-07 16:13_

---

_Comment by @charliermarsh on 2022-10-07 16:14_

[v0.0.58](https://github.com/charliermarsh/ruff/releases/tag/v0.0.58) is building now.

---

_Renamed from "ruff's `--fix` return a non-zero exit code even though there's only one fixable lint error." to "ruff's `--fix` returns a non-zero exit code even though there's only one fixable lint error." by @snapdgn on 2022-10-07 16:19_

---
