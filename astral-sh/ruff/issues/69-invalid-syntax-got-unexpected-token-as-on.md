```yaml
number: 69
title: "`Invalid syntax. Got unexpected token 'as'` on multiple `... as ...` inside a with statement"
type: issue
state: closed
author: Greesb
labels: []
assignees: []
created_at: 2022-09-01T12:02:20Z
updated_at: 2022-09-01T13:13:20Z
url: https://github.com/astral-sh/ruff/issues/69
synced_at: 2026-01-10T15:56:04Z
```

# `Invalid syntax. Got unexpected token 'as'` on multiple `... as ...` inside a with statement

---

_Issue opened by @Greesb on 2022-09-01 12:02_

Found this bug while testing ruff on my code, here is a simplified example to reproduce: 

`main.py`:
```python
with (
    open("test", "r") as f,
    open("test2", "r") as f2,
):
    print("hello")
```

ruff output:
```
[2022-09-01][14:00:00][ruff::pyproject][DEBUG] Found pyproject.toml at: pyproject.toml
[2022-09-01][14:00:00][ruff][DEBUG] Identified files to lint in: 3.25µs
[2022-09-01][14:00:00][ruff][ERROR] Failed to check main.py: invalid syntax. Got unexpected token 'as' at line 2 column 23
[2022-09-01][14:00:00][ruff][DEBUG] Checked files in: 174.625µs
Found 0 error(s).
```

---

_Renamed from "`Invalid syntax. Got unexpected token 'as'` on multiple `.. as ...` inside a with statement" to "`Invalid syntax. Got unexpected token 'as'` on multiple `... as ...` inside a with statement" by @Greesb on 2022-09-01 12:02_

---

_Comment by @charliermarsh on 2022-09-01 13:10_

Thanks! I think this is a dupe of #54.

---

_Closed by @charliermarsh on 2022-09-01 13:13_

---
