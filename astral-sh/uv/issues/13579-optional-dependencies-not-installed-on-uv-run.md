```yaml
number: 13579
title: "Optional dependencies not installed on `uv run --extra`"
type: issue
state: closed
author: tomaszbk
labels:
  - bug
assignees: []
created_at: 2025-05-21T15:16:34Z
updated_at: 2025-05-21T15:37:46Z
url: https://github.com/astral-sh/uv/issues/13579
synced_at: 2026-01-12T16:01:32Z
```

# Optional dependencies not installed on `uv run --extra`

---

_@tomaszbk_

### Summary

Issue: running `uv run <file> --extra <extra>` isnt working


How to reproduce:
1- Create empty folder
2- Create this pyproject.toml:
```
[project]
name= "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12.10"
dependencies = []

[project.optional-dependencies]
cpu = [
  "fastapi>=0.115.12",
]

```

3- Create this main.py:
```python
import fastapi
```

4- Run `uv run main.py --extra cpu`
Output: `ModuleNotFoundError: No module named 'fastapi'`



### Platform

windows 11

### Version

0.7.6

### Python version

3.12.10

---

_Label `bug` added by @tomaszbk on 2025-05-21 15:16_

---

_Renamed from "Package dependencies not installed on `uv run --extra`" to "Dependencies not installed on `uv run --extra`" by @tomaszbk on 2025-05-21 15:17_

---

_Renamed from "Dependencies not installed on `uv run --extra`" to "Optional dependencies not installed on `uv run --extra`" by @tomaszbk on 2025-05-21 15:20_

---

_Comment by @tomaszbk on 2025-05-21 15:20_

normal dependencies are installed, but not [project.optional-dependencies]

---

_Comment by @charliermarsh on 2025-05-21 15:33_

You need to pass the extra flag _before_ main.py. As-is, it’s interpreted as an argument to main.py, not uv run.

---

_Comment by @tomaszbk on 2025-05-21 15:37_

Thanks for the quick reply! this solver my issue !

---

_Closed by @tomaszbk on 2025-05-21 15:37_

---
