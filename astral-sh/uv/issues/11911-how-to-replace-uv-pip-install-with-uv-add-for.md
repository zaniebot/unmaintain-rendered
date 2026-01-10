---
number: 11911
title: How to replace uv pip install with uv add for installing PyTorch?
type: issue
state: closed
author: wzr0108
labels:
  - question
assignees: []
created_at: 2025-03-03T02:58:10Z
updated_at: 2025-03-03T06:52:29Z
url: https://github.com/astral-sh/uv/issues/11911
synced_at: 2026-01-10T01:25:12Z
---

# How to replace uv pip install with uv add for installing PyTorch?

---

_Issue opened by @wzr0108 on 2025-03-03 02:58_

### Question

I'm trying to install PyTorch using uv add, but I'm not sure how to correctly replace the following uv pip install command:
```
uv pip install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu121
```

### Platform

Ubuntu 20.04.2 LTS

### Version

uv 0.6.3

---

_Label `question` added by @wzr0108 on 2025-03-03 02:58_

---

_Comment by @wzr0108 on 2025-03-03 03:08_

My current approach is to run:
```
uv pip install torch==2.3.1 --index-url https://download.pytorch.org/whl/cu121
uv add torch==2.3.1
```
and manually modify pyproject.toml.
This process is a bit cumbersome. I'd like to know if there's a simpler way to achieve this.

---

_Comment by @charliermarsh on 2025-03-03 03:54_

I would expect:

```
uv add torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index torch=https://download.pytorch.org/whl/cu121
```

---

_Comment by @wzr0108 on 2025-03-03 05:48_

When I run:
```
uv add torch==2.2.2 --index torch=https://download.pytorch.org/whl/cu118
```
I encounter the following error:
```
Resolved 22 packages in 7.60s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cu118` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.10 (`cp310`), but `markupsafe` (v3.0.2) only has wheels with the following Python implementation tag: `cp313`
```

---

_Comment by @wzr0108 on 2025-03-03 06:52_

I added the following to my pyproject.toml file:
```
[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
torchvision = [
  { index = "pytorch-cu118", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
```
Then, I ran:
```
uv add torch==2.2.2
```
This successfully resolved the issue! 

---

_Closed by @wzr0108 on 2025-03-03 06:52_

---
