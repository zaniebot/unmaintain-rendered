```yaml
number: 16767
title: UV does not work with detectron
type: issue
state: closed
author: 1uck13ss
labels:
  - needs-mre
assignees: []
created_at: 2025-11-18T09:59:26Z
updated_at: 2025-12-03T05:55:39Z
url: https://github.com/astral-sh/uv/issues/16767
synced_at: 2026-01-12T16:02:38Z
```

# UV does not work with detectron

---

_@1uck13ss_

### Summary

I have an issue with UV and detectron2, when I ran UV with this pyproject.toml as suggested by https://github.com/astral-sh/uv/issues/16243, it did not work. I tried a simple test by importing detectron2 but i encountered the error torch not found even though torch is installed.  

```[project]
name = "balance"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "torch",
    "torchvision",
    "detectron2 @ git+https://github.com/facebookresearch/detectron2.git",
]

[tool.uv.extra-build-dependencies]
detectron2 = [{ requirement = "torch", match-runtime = true }]

[[tool.uv.index]]
name = "pypi"
url  = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "pytorch-cu128"
url  = "https://download.pytorch.org/whl/cu128"

[tool.uv.sources]
torch = { index = "pytorch-cu128" }
torchvision = { index = "pytorch-cu128" }
```

here are my environments
torch==2.9.1
torchaudio==2.5.0
torchvision==0.20.0
traitlets==5.14.3
typing-extensions==4.15.0
wcwidth==0.2.14
wheel==0.45.1

### Platform

Windows 11

### Version

0.9.10

### Python version

Python 3.10.19

---

_Label `bug` added by @1uck13ss on 2025-11-18 09:59_

---

_Comment by @konstin on 2025-11-18 10:34_

What command did you run, and what error message did you get?

---

_Label `bug` removed by @charliermarsh on 2025-11-28 13:34_

---

_Label `needs-mre` added by @charliermarsh on 2025-11-28 13:34_

---

_Comment by @charliermarsh on 2025-12-03 05:55_

Closing due to lack of MRE but can always re-open.

---

_Closed by @charliermarsh on 2025-12-03 05:55_

---
