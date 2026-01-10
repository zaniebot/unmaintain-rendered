```yaml
number: 11776
title: Platform specific indexes not working
type: issue
state: closed
author: FredrikNoren
labels:
  - bug
assignees: []
created_at: 2025-02-25T10:18:04Z
updated_at: 2025-02-27T02:46:39Z
url: https://github.com/astral-sh/uv/issues/11776
synced_at: 2026-01-10T03:50:31Z
```

# Platform specific indexes not working

---

_Issue opened by @FredrikNoren on 2025-02-25 10:18_

### Summary

This `pyproject.toml` fails when I run uv sync:

```
[project]
name = "uvtest"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["torch>=2.6.0", "torchvision>=0.21.0"]

[tool.uv.sources]
torch = { index = "pytorch_cu124", marker = "sys_platform == 'linux'" }
torchvision = { index = "pytorch_cu124", marker = "sys_platform == 'linux'" }

[[tool.uv.index]]
name = "pytorch_cu124"
url = "https://download.pytorch.org/whl/cu124"
```

The error is:
```
uvtest git:(master) ✗ uv sync
Resolved 30 packages in 1.77s
error: Found duplicate package `torchvision==0.21.0+cu124 @ registry+https://download.pytorch.org/whl/cu124`
```

### Platform

Darwin 23.6.0 arm64

### Version

v0.6.3

### Python version

Python 3.12.4

---

_Label `bug` added by @FredrikNoren on 2025-02-25 10:18_

---

_Comment by @charliermarsh on 2025-02-25 14:30_

Strange, thanks.

---

_Comment by @charliermarsh on 2025-02-25 22:04_

For what it's worth, I think you want `explicit = true` on that index (which does work as expected):

```toml
[project]
name = "uvtest"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["torch>=2.6.0", "torchvision>=0.21.0"]

[tool.uv.sources]
torch = { index = "pytorch_cu124", marker = "sys_platform == 'linux'" }
torchvision = { index = "pytorch_cu124", marker = "sys_platform == 'linux'" }

[[tool.uv.index]]
name = "pytorch_cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-26 01:14_

---

_Comment by @charliermarsh on 2025-02-26 02:47_

I've identified the problem and have some ideas on how to fix it.

---

_Closed by @charliermarsh on 2025-02-27 02:46_

---
