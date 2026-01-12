```yaml
number: 12460
title: "There is no available `triton` in windows but it required when I download torch==2.1.0+cu118"
type: issue
state: closed
author: MrXnneHang
labels:
  - bug
assignees: []
created_at: 2025-03-25T10:25:16Z
updated_at: 2025-03-25T10:49:54Z
url: https://github.com/astral-sh/uv/issues/12460
synced_at: 2026-01-12T16:01:03Z
```

# There is no available `triton` in windows but it required when I download torch==2.1.0+cu118

---

_@MrXnneHang_

### Summary

```toml
[project]
name = "Auto-Caption-Generate-Offline"
version = "2.4.0"
requires-python = "==3.11.11"
dependencies = [
    "torch==2.1.0",
    "torchaudio==2.1.0",

]

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", marker = "platform_system != 'Darwin'"  },
]
torchaudio = [
  { index = "pytorch-cu118", marker = "platform_system != 'Darwin'"  },
]

```

error:

```shell
PS D:\program\Auto-Caption-Generator-Offline> uv sync
Using CPython 3.11.11
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 13 packages in 2.13s
error: Distribution `triton==2.1.0 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `triton` (v2.1.0) only has wheels for the following platforms: `manylinux_2_17_x86_64`, `manylinux2014_x86_64`
```

list of tritons: [https://download.pytorch.org/whl/triton/](https://download.pytorch.org/whl/triton/)

### Platform

windows11 x86_64

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

3.11.11

---

_Label `bug` added by @MrXnneHang on 2025-03-25 10:25_

---

_Renamed from "There was no available `triton` in windows but it required when I download torch==2.1.0+cu118" to "There is no available `triton` in windows but it required when I download torch==2.1.0+cu118" by @MrXnneHang on 2025-03-25 10:27_

---

_Comment by @MrXnneHang on 2025-03-25 10:29_

![Image](https://github.com/user-attachments/assets/1f3f1191-dbb1-4589-847a-affa6b45fa58)

It works well in my deepin23

---

_Comment by @MrXnneHang on 2025-03-25 10:47_

It seems to only happen in `torch==2.1.0`, when I upgrade to `2.2.0`, It can be solved.

---

_Closed by @MrXnneHang on 2025-03-25 10:49_

---
