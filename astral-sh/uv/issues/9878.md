```yaml
number: 9878
title: PyTorch install fails on M1 using a Debian container ?
type: issue
state: closed
author: santi-mir
labels:
  - question
assignees: []
created_at: 2024-12-13T19:37:26Z
updated_at: 2024-12-15T16:17:23Z
url: https://github.com/astral-sh/uv/issues/9878
synced_at: 2026-01-10T04:36:21Z
```

# PyTorch install fails on M1 using a Debian container ?

---

_Issue opened by @santi-mir on 2024-12-13 19:37_

Here trying to set up the pyproject to install torch. Currently I just tried installing within a Debian Container, on an M1. 

But [it fails with the example in the docs.](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies)

**Minimal Context**

uv version:

```bash
$ uv --version
uv 0.5.8
```

Config from the guide (simplified it a bit; just removed cuda and torchvision.):

```toml
[project.optional-dependencies]
cpu = [ "torch>=2.5.1"]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

I've clean the cache and started a fresh venv.

**Issue**

And then running:


```bash
vscode ➜ /workspaces/project (dev) $ uv venv
Using CPython 3.10.16
Creating virtual environment at: .venv
vscode ➜ /workspaces/project (dev) $ source .venv/bin/activate
(project) vscode ➜ /workspaces/project (dev) $ uv sync --extra cpu
Resolved 85 packages in 6.05s
error: Distribution `torch==2.5.1+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```


I may be doing something wrong, but can not figure it out. 

---

_Renamed from "PyTorch install fails on MacOS?" to "PyTorch install fails on M1 using a Debian container ?" by @santi-mir on 2024-12-13 19:56_

---

_Comment by @charliermarsh on 2024-12-13 20:38_

Umm. It might think that you're on an ARM Linux machine? In which case, `2.5.1+cpu` isn't right. You'd want `torch===2.5.1`, or like `marker = "platform_system != 'Darwin' and platform_machine != 'arm64'"`?

---

_Label `question` added by @charliermarsh on 2024-12-13 20:38_

---

_Comment by @zanieb on 2024-12-13 20:49_

Or `docker run --platform linux/x86_64` if you don't want an arm64 image?

---

_Closed by @santi-mir on 2024-12-13 21:22_

---

_Comment by @santi-mir on 2024-12-15 16:13_

Just to add:

> `marker = "platform_system != 'Darwin' and platform_machine != 'arm64'"`

I used _aarch64_ instead because _arm64_ was failing (or that is my impression at least.)

Now for linux-aarch64 on MacOS fallbacks to PyTorch PyPI index for [manylinux-aarch64](https://pypi.org/project/torch/#files).

And this one:

> docker run --platform linux/x86_64 if you don't want an arm64 image?

doesn't seem [the best option for my usecase.](https://github.com/pytorch/pytorch/issues/81224) It's even slower than the non-mps aarm64 case is already.

One can not use MPS in Docker, but that's not uv's problem. 

Thanks again @charliermarsh @zanieb 

---
