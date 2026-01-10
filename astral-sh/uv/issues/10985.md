```yaml
number: 10985
title: multiple versions of the same package can be installed in some cases when using conflicting extras
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2025-01-27T14:05:13Z
updated_at: 2025-01-29T22:21:12Z
url: https://github.com/astral-sh/uv/issues/10985
synced_at: 2026-01-10T03:50:31Z
```

# multiple versions of the same package can be installed in some cases when using conflicting extras

---

_Issue opened by @BurntSushi on 2025-01-27 14:05_

> I found the problem by trying to reproduce the problem on a new repo. The problem is with package torchmetrics. When it is in the workspace package/dependency this case exists. I have yet not inspect what can be wrong with that package configuration.
> 
> Minimal example:
> 
> Child:
> ```
> [project]
> name = "child-surface"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.12"
> dependencies = [
>     "torchmetrics>=1.1.0",
> ]
> 
> 
> [project.optional-dependencies]
> cpu = [
>     "torch==2.4.1", 
>     "torchvision==0.19.1"
> ]
> gpu = [
>     "torch==2.4.1", 
>     "torchvision==0.19.1",
>     "nvidia-dali-cuda120==1.30.0",
> ]
> [tool.uv]
> conflicts = [
>   [
>     { extra = "cpu" },
>     { extra = "gpu" },
>   ],
> ]
> 
> [tool.uv.sources]
> torch = [
>   { index = "pytorch-cpu", extra = "cpu" },
>   { index = "pytorch-cu124", extra = "gpu" },
> ]
> torchvision = [
>   { index = "pytorch-cpu", extra = "cpu" },
>   { index = "pytorch-cu124", extra = "gpu" },
> ]
> [[tool.uv.index]]
> name = "pytorch-cpu"
> url = "https://download.pytorch.org/whl/cpu"
> explicit = true
> 
> [[tool.uv.index]]
> name = "pytorch-cu124"
> url = "https://download.pytorch.org/whl/cu124"
> explicit = true
> ```
> 
> Parent:
> ```
> [project]
> name = "parent-surface"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.12"
> dependencies = []
> 
> [tool.uv.sources]
> child-surface = [{ workspace = true  },]
> 
> [tool.uv.workspace]
> members = ["child-surface"]
> 
> 
> [project.optional-dependencies]
> cpu = [
>     "child-surface[cpu]>=0.0.1", 
> ]
> gpu = [
>     "child-surface[gpu]>=0.0.1", 
> ]
> 
> [tool.uv]
> conflicts = [
>   [
>     { extra = "cpu" },
>     { extra = "gpu" },
>   ],
> ]
> ```
>  
> Full zipped example with lock file:
> [parent-surface.zip](https://github.com/user-attachments/files/18557374/parent-surface.zip)
> 
> ---
> Edit: Looking at their repo and build system in [setup.py](https://github.com/Lightning-AI/torchmetrics/blob/master/setup.py#L162) I see that they define [base requirements](https://github.com/Lightning-AI/torchmetrics/blob/master/requirements/base.txt#L6C1-L6C22) which is just `torch >=2.0.0, <2.7.0`
> 
> So uv resolver has installed `torch==2.4.1+cu124` and then installs `torch==2.4.1` which is strange having two different versions of the same package
> 
> This behavior changed after introducing this change 

 _Originally posted by @mchalecki in [#10590](https://github.com/astral-sh/uv/issues/10590#issuecomment-2615516581)_

---

_Label `bug` added by @BurntSushi on 2025-01-27 14:05_

---

_Comment by @BurntSushi on 2025-01-27 14:11_

After creating the above two `pyproject.toml` files (`pyproject.toml` and `child-surface/pyproject.toml`), just run `uv sync --extra cpu` and I at least get (on Linux):

```
 + torch==2.4.1
 + torch==2.4.1+cpu
```

The same thing happens for `uv sync --extra gpu`, but you get `torch==2.4.1+cu124` instead.

My guess here is that when I relaxed the error checking around unconditional enabling of conflicting extras in #10875, this may have uncovered a new bug in how forks are created and managed for conflicting extras? Not sure. But the relevant lines in the lock file are:

```toml
[[package]]
name = "torchmetrics"
version = "1.6.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "lightning-utilities" },
    { name = "numpy" },
    { name = "packaging" },
    { name = "torch", version = "2.4.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(platform_machine == 'aarch64' and sys_platform == 'linux' and extra == 'extra-13-child-surface-cpu') or (platform_machine != 'aarch64' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (platform_machine != 'aarch64' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (sys_platform == 'darwin' and extra == 'extra-13-child-surface-cpu') or (sys_platform != 'linux' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (sys_platform != 'linux' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (sys_platform == 'darwin' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (extra != 'extra-13-child-surface-cpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu')" },
    { name = "torch", version = "2.4.1", source = { registry = "https://download.pytorch.org/whl/cu124" }, marker = "(platform_machine == 'aarch64' and sys_platform == 'linux' and extra == 'extra-13-child-surface-gpu') or (platform_machine != 'aarch64' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (platform_machine != 'aarch64' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (sys_platform != 'linux' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (sys_platform != 'linux' and extra != 'extra-13-child-surface-cpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (extra != 'extra-13-child-surface-gpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu')" },
    { name = "torch", version = "2.4.1", source = { registry = "https://pypi.org/simple" }, marker = "(extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (extra != 'extra-13-child-surface-cpu' and extra != 'extra-13-child-surface-gpu')" },
    { name = "torch", version = "2.4.1+cpu", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(platform_machine != 'aarch64' and sys_platform == 'linux' and extra == 'extra-13-child-surface-cpu') or (sys_platform != 'darwin' and sys_platform != 'linux' and extra == 'extra-13-child-surface-cpu') or (sys_platform == 'darwin' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (sys_platform == 'linux' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (sys_platform == 'darwin' and extra != 'extra-13-child-surface-gpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (sys_platform == 'linux' and extra == 'extra-13-child-surface-cpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu') or (extra != 'extra-13-child-surface-cpu' and extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu')" },
    { name = "torch", version = "2.4.1+cu124", source = { registry = "https://download.pytorch.org/whl/cu124" }, marker = "(platform_machine != 'aarch64' and extra == 'extra-13-child-surface-gpu') or (sys_platform != 'linux' and extra == 'extra-13-child-surface-gpu') or (extra == 'extra-13-child-surface-cpu' and extra == 'extra-13-child-surface-gpu') or (extra == 'extra-14-parent-surface-cpu' and extra == 'extra-14-parent-surface-gpu')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/14/c5/8d916585d4d6eb158105c21b28cd4b0ed296d74e499bf8f104368de16619/torchmetrics-1.6.1.tar.gz", hash = "sha256:a5dc236694b392180949fdd0a0fcf2b57135c8b600e557c725e077eb41e53e64", size = 540022 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/9d/e1/84066ff60a20dfa63f4d9d8ddc280d5ed323b7f06504dbb51c523b690116/torchmetrics-1.6.1-py3-none-any.whl", hash = "sha256:c3090aa2341129e994c0a659abb6d4140ae75169a6ebf45bffc16c5cb553b38e", size = 927305 },
]
```

The conflict markers are way too complicated me to digest at a glance here, so I think this one requires a bit more investigation.

---

_Comment by @BurntSushi on 2025-01-27 14:12_

And note that the conflicting extras defined in the _parent_ package here are superfluous. If I remove them, the bug remains.

---

_Renamed from "multiple versions of the same package can be installed in some cases when using conflicting dependencies" to "multiple versions of the same package can be installed in some cases when using conflicting extras" by @BurntSushi on 2025-01-27 14:13_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-01-27 14:20_

---

_Closed by @BurntSushi on 2025-01-29 22:21_

---

_Closed by @BurntSushi on 2025-01-29 22:21_

---
