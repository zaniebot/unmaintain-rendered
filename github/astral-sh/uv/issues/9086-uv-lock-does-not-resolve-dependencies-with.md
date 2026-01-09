---
number: 9086
title: "uv lock does not resolve dependencies with multiple indexes and `extra` markers as expected"
type: issue
state: closed
author: Karlinator
labels: []
assignees: []
created_at: 2024-11-13T15:43:18Z
updated_at: 2024-11-29T13:11:33Z
url: https://github.com/astral-sh/uv/issues/9086
synced_at: 2026-01-07T13:12:18-06:00
---

# uv lock does not resolve dependencies with multiple indexes and `extra` markers as expected

---

_Issue opened by @Karlinator on 2024-11-13 15:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv 0.5.1, python 3.13, Fedora 41.

I'm trying to set up a project such that I can install it in three different ways:
1. Without pytorch
2. With pytorch without cuda support
3. With pytorch on cuda

I've tried roughly 5000 different things without success. Here's where I'm at now:

```toml

[dependency-groups]
torch = [
    "torch==2.5.1+cpu; extra != 'cuda'",
    "torch==2.5.1+cu124; extra == 'cuda'",
    "sentence-transformers>=3.3.0"
]


[tool.uv.sources]
torch = [
    { index = "torch-gpu", marker = "extra == 'cuda'" },
    { index = "torch-cpu", marker = "extra != 'cuda'" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

However, the lockfile seems to have not considered the split on `extra` here, and only resolves either the cpu variant or the cuda variant (in this exact axample it's the cpu variant, but messing around with it a little you can get the cuda variant instead). So doing `uv sync --group torch --extra cuda` and `uv sync --group torch`  gives the exact same result.

The first part of the lockfile looks correct (tying the specifier and index to the marker), but then it only resolves one version of torch, and that's 2.5.1+cpu with cpu wheels and missing the cuda dependencies.

```toml
[package.metadata.requires-dev]
torch = [
    { name = "sentence-transformers", specifier = ">=3.3.0" },
    { name = "torch", marker = "extra == 'cuda'", specifier = "==2.5.1+cu124", index = "https://download.pytorch.org/whl/cu124" },
    { name = "torch", marker = "extra != 'cuda'", specifier = "==2.5.1+cpu", index = "https://download.pytorch.org/whl/cpu" },
]

[[package]]
name = "torch"
version = "2.5.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "setuptools", marker = "python_full_version >= '3.12'" },
    { name = "sympy" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-linux_x86_64.whl", hash = "sha256:07d7c9e069123d5af08b0cf0013d74f680b2d8be7d9e2cf561a52c90c55d9409" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-win_amd64.whl", hash = "sha256:81531d4d5ca74163dc9574b87396531e546a60cceb6253303c7db6a21e867fdf" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp312-cp312-linux_x86_64.whl", hash = "sha256:4856f9d6925121d13c2df07aa7580b767f449dfe71ae5acde9c27535d5da4840" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp312-cp312-win_amd64.whl", hash = "sha256:a6b720410350765d3d77c01a5ce098a6c45af446284e45e87a98b8a16e7d564d" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp313-cp313-linux_x86_64.whl", hash = "sha256:5dbbdf83caa90d0bcaa50e4933ca424889133b35226db79000877d4ec5d9ea37" },
]
```

Looking at the output of `uv -v lock`, it only looks for a compatible version of torch once, and it finds the cpu version only. It doesn't split on the "extra" marker to look in the separate pinned indexes.

```
DEBUG Searching for a compatible version of torch (>=1.11.0)
DEBUG Selecting: torch==2.5.1+cpu [compatible] (torch-2.5.1+cpu-cp311-cp311-linux_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=07d7c9e069123d5af08b0cf0013d74f680b2d8be7d9e2cf561a52c90c55d9409
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=07d7c9e069123d5af08b0cf0013d74f680b2d8be7d9e2cf561a52c90c55d9409
DEBUG Found not-modified response for: https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-linux_x86_64.whl#sha256=07d7c9e069123d5af08b0cf0013d74f680b2d8be7d9e2cf561a52c90c55d9409
DEBUG Pre-fork all marker environments took 0.937s
DEBUG Splitting resolution on torch==2.5.1+cpu over setuptools into 2 resolution with separate markers
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Adding transitive dependency for torch==2.5.1+cpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+
DEBUG Adding transitive dependency for torch==2.5.1+cpu: typing-extensions>=4.8.0
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Adding transitive dependency for torch==2.5.1+cpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+cpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+
DEBUG Adding transitive dependency for torch==2.5.1+cpu: typing-extensions>=4.8.0
DEBUG Solving split (python_full_version < '3.12') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.11" }]), range: RequiresPythonRange(LowerBound(Included("3.11")), UpperBound(Excluded("3.12"))
) })
```

I've also tried a variety of extras-based approaches. Commonly, these fail to resolve because even with the sources spec uv goes to pypi to find the metadata, or they end up in the same sitation as above that it just always resolves one or the other.

Is there some way I'm supposed to do this that I haven't found, or is this not fully supported?

I know that this is pytorch being very extra, and that I can get around this by doing `uv pip install torch --index-url="https://downloads.pytorch.org/whl/cpu` and stuff, but that loses out on the benefits of the uv lockfile and all that.

---

_Comment by @charliermarsh on 2024-11-13 15:45_

That's correct. We don't support this right now. You should follow the conflicting extras work.

---

_Comment by @charliermarsh on 2024-11-13 15:45_

See: https://github.com/astral-sh/uv/pull/8976

---

_Comment by @charliermarsh on 2024-11-13 15:47_

Specifically, we don't allow you to ask for conflicting versions of a package based on the presence of an extra. In the (near) future, you can mark extras as mutually incompatible, which will let you do what you want here without trouble.

---

_Comment by @charliermarsh on 2024-11-13 15:47_

The relevant issue to follow is actually here: https://github.com/astral-sh/uv/issues/6981. I'm gonna merge into that.

---

_Closed by @charliermarsh on 2024-11-13 15:47_

---

_Comment by @Karlinator on 2024-11-14 16:37_

Man, coming back here the next day to see both the issues you linked have been merged is so cool. You guys rock!

---

_Comment by @Karlinator on 2024-11-15 12:10_

This still doesn't seem to be working fully. Using 0.5.2 I still only get one index queried. I've specified:

```
[project.optional-dependencies]
cpu = [
    "torch==2.5.1+cpu",
    "sentence-transformers>=3.3.0"
]
gpu = [
    "torch==2.5.1",
    "sentence-transformers>=3.3.0"
]

[tool.uv.sources]
torch = [
    { index = "torch-cpu", marker = "extra != 'gpu' and extra == 'cpu'" },
    { index = "torch-gpu", marker = "extra == 'gpu' and extra != 'cpu'" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[tool.uv]
conflicts = [
    [
        { extra = "cpu" },
        { extra = "gpu" },
    ],
]
```

And now it fails to resolve dependencies because it can't find `2.5.1+cpu` in the `https://download.pytorch.org/whl/cu124` index. If I remove the `+cpu` it just installs the cuda version for both `uv sync --extra cpu` and `uv sync --extra cpu`.

---

_Comment by @charliermarsh on 2024-11-15 13:52_

@Karlinator -- I think there's specific work that needs to happen atop #6981 to respect extras in `tool.uv.sources` like that. I would be the natural owner for that work. I'll file an issue later today when I have some time to refresh my memory.

---

_Comment by @ew-pallon on 2024-11-15 17:48_

I have the exact same use case as @Karlinator, which is why I was quite excited about the new features for conflicting extras/groups. I would greatly appreciate it if you could make this work @charliermarsh  --then my team could finally switch to uv! 😊 

---

_Comment by @charliermarsh on 2024-11-15 19:06_

Totally hear you. We definitely want to support this use-case.

---

_Comment by @charliermarsh on 2024-11-19 19:41_

This now works in v0.5.3:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

Docs on PyTorch [here](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies).

---
