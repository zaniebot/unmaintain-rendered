```yaml
number: 16685
title: Resolution problem with pyproject.toml tool.uv.index for torch / torchcodec / torchvision
type: issue
state: open
author: mscheltienne
labels:
  - bug
  - external
assignees: []
created_at: 2025-11-11T11:34:27Z
updated_at: 2025-11-28T15:00:49Z
url: https://github.com/astral-sh/uv/issues/16685
synced_at: 2026-01-12T16:02:36Z
```

# Resolution problem with pyproject.toml tool.uv.index for torch / torchcodec / torchvision

---

_@mscheltienne_

### Summary

With this minimal `pyproject.toml`:

```
[project]
dependencies = []
description = 'Test torch index.'
name = 'test'
readme = 'README.md'
requires-python = '>=3.12'
version = '0.1.0'

[project.optional-dependencies]
cpu = ['torch', 'torchcodec', 'torchvision']
cu128 = ['torch', 'torchcodec', 'torchvision']

[tool.uv]
conflicts = [[{extra = 'cpu'}, {extra = 'cu128'}]]

[[tool.uv.index]]
explicit = true
name = 'torch-cpu'
url = 'https://download.pytorch.org/whl/cpu'

[[tool.uv.index]]
explicit = true
name = 'torch-cu128'
url = 'https://download.pytorch.org/whl/cu128'

[tool.uv.sources]
torch = [
  {extra = 'cpu', index = 'torch-cpu'},
  {extra = 'cu128', index = 'torch-cu128'},
]
torchcodec = [
  {extra = 'cpu', index = 'torch-cpu'},
  {extra = 'cu128', index = 'torch-cu128'},
]
torchvision = [
  {extra = 'cpu', index = 'torch-cpu'},
  {extra = 'cu128', index = 'torch-cu128'},
]
``` 

The command `uv sync --extra cpu` fails with: 

```
error: Distribution `torchvision==0.24.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_42_x86_64`), but `torchvision` (v0.24.0) only has wheels for the following platforms: `manylinux_2_28_aarch64`, `macosx_11_0_arm64`; consider adding "sys_platform == 'linux' and platform_machine == 'x86_64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

While running a manual venv and install of the packages works:

```
❯ uv venv --python 3.12
Using CPython 3.12.12 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
~/Downloads/test is 📦 v0.1.0 via 🐍 v3.14.0 on ☁️  dandelion-admin (us-east-1) 
❯ uv pip install torch torchcodec torchvision --index-url https://download.pytorch.org/whl/cpu
Resolved 14 packages in 1.29s
Prepared 3 packages in 270ms
Installed 14 packages in 386ms
 + filelock==3.19.1
 + fsspec==2025.9.0
 + jinja2==3.1.6
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.5
 + numpy==2.3.3
 + pillow==11.3.0
 + setuptools==70.2.0
 + sympy==1.14.0
 + torch==2.9.0+cpu
 + torchcodec==0.8.1
 + torchvision==0.24.0+cpu
 + typing-extensions==4.15.0
``` 

### Platform

Fedora 43

### Version

uv 0.9.8

### Python version

Python 3.13.9 or Python 3.12.12

---

_Label `bug` added by @mscheltienne on 2025-11-11 11:34_

---

_Comment by @mscheltienne on 2025-11-11 11:36_

Note that a compatible wheel does exist on the index: [https://download.pytorch.org/whl/cpu/torchvision-0.24.0%2Bcpu-cp312-cp312-manylinux_2_28_x86_64.whl](https://download.pytorch.org/whl/cpu/torchvision-0.24.0%2Bcpu-cp312-cp312-manylinux_2_28_x86_64.whl)

```
❯ uv pip show torchvision
Name: torchvision
Version: 0.24.0+cpu
Location: /home/mscheltienne/Downloads/test/.venv/lib/python3.12/site-packages
Requires: numpy, pillow, torch
Required-by:
``` 

Wheel details:

```
  - Package: torchvision==0.24.0+cpu
  - Python version: 3.12 (cp312)
  - Platform: Linux x86_64
  - Glibc: manylinux_2_28 (compatible with my manylinux_2_42 system)
  - SHA256: baa37f80155a3b055911d5c2418be1e16b95bb69f671dbb0d07d67504093e23d
```

---

_Comment by @lagamura on 2025-11-16 19:39_

Faced something similar, and used the following workaround:

```toml
[tool.uv]
required-environments = ["sys_platform == 'linux' and platform_machine == 'x86_64'"]
```

You can require multiple pairs of `sys_platform` and `platform_machine` architecture, if you want to higher the minimum supported scenarios. e.g.

```toml
[tool.uv]
required-environments = [
    "((sys_platform == 'linux' and platform_machine == 'x86_64') or (sys_platform == 'darwin') or (sys_platform == 'win32'))"
]
```

docs ref for required_environments: https://docs.astral.sh/uv/concepts/resolution/#required-environments

---

_Comment by @charliermarsh on 2025-11-28 14:39_

I believe this is ultimately due to https://github.com/pytorch/vision/issues/9249.

---

_Comment by @charliermarsh on 2025-11-28 14:39_

In short, there's inconsistency around the use of the `+cpu` specifier for that package and version, which is confusing the uv resolver.

---

_Label `external` added by @konstin on 2025-11-28 15:00_

---
