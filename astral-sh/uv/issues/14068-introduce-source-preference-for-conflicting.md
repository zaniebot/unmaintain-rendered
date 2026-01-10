---
number: 14068
title: Introduce source preference for conflicting dependencies
type: issue
state: open
author: j-adamczyk
labels:
  - enhancement
assignees: []
created_at: 2025-06-16T07:56:08Z
updated_at: 2025-06-28T13:26:37Z
url: https://github.com/astral-sh/uv/issues/14068
synced_at: 2026-01-10T01:25:42Z
---

# Introduce source preference for conflicting dependencies

---

_Issue opened by @j-adamczyk on 2025-06-16 07:56_

### Summary

When declaring conflicts for multiple optional groups, I want to be able to select the order of preference. If then I want to install 2 groups with conflicting dependency, `uv` will select and install one that is more preferred. Adding an option to CLI for `--all-extras` to specify this would also be ok.

Use case is ML with PyTorch. I have multiple groups, e.g. `preprocessing`, `training`, `inference`. Only `training` uses GPU, all others use CPU. They need to be separated to enable building separate Docker containers. For development, I need to install everything locally, but with GPU. Currently, I have to specify separate options. With a lot of groups, this gets pretty inconvenient.

With current `uv` state, I have two options. First, push all PyTorch to separate optional group and install it separately. With many groups, this results in PyTorch far from the actual dependency group, making `pyproject.toml` hard to read. Also, I have to pass `--extra` to each group, and need to somehow specify what group uses what version, e.g. search with regex for "train" in dependency group name, which is really not intuitive nor readable. Second option is to use `extra` markers, e.g. specify dependency as `"torch==2.2.*; extra == 'pytorch-cpu'", add source, index, conflicts etc. as [in uv docs](https://docs.astral.sh/uv/guides/integration/pytorch/). Here, the problem is that I can't install all dependency groups at the same time, as PyTorch has conflict between groups.

I basically want to translate the Poetry workflow from https://github.com/python-poetry/poetry/issues/6409#issuecomment-1911735833. My Poetry `pyproject.toml` is:
```
[tool.poetry.group.train_model.dependencies]
torch = {version = "2.2.*", source = "pytorch-gpu", markers = "extra!='pytorch-cpu'"}

[tool.poetry.group.inference.dependencies]
torch = {version = "2.2.*", source = "pytorch-cpu", markers = "extra=='pytorch-cpu'"}

[tool.poetry.extras]
pytorch-cpu = []

[[tool.poetry.source]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
priority = "explicit"

[[tool.poetry.source]]
name = "pytorch-gpu"
url = "https://download.pytorch.org/whl/cu121"
priority = "explicit"
```

In Poetry, when I do `poetry sync`, it will prefer CPU version. However, if I only install `train_model` group, it will pick up the GPU source.

### Example

_No response_

---

_Label `enhancement` added by @j-adamczyk on 2025-06-16 07:56_

---

_Comment by @konstin on 2025-06-27 13:27_

What about the following example from the docs:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch>=2.7.0",
  "torchvision>=0.22.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
  { index = "pytorch-cu128", marker = "sys_platform == 'linux'" },
]
torchvision = [
  { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
  { index = "pytorch-cu128", marker = "sys_platform == 'linux'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

Additionally, you can include one extra in another extra:

```toml
[project]
name = "foo"
version = "0.1.0"

[project.optional-dependencies]
a = ["numpy"]
b = ["scipy", "foo[a]"]
```


---

_Comment by @j-adamczyk on 2025-06-28 12:17_

@konstin this won't work, as my marker is not an OS. All my machines run Linux, but either with CPU or GPU.

---

_Comment by @charliermarsh on 2025-06-28 12:50_

Am I right that this is the same as the optional dependencies example in the [docs](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies), except that you want the CPU group to be used by default?

---

_Comment by @j-adamczyk on 2025-06-28 12:58_

Similar, but not exactly the same. Coming to the [docs example](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-environment-markers), I basically want:
1. `uv sync` - installs all packages from all groups, including GPU PyTorch (note that one group may have CPU, and another GPU PyTorch)
2. Install a particular group - installs a PyTorch version defined for that specific group

The first point is the main motivation for this issue. My Poetry setup did exactly this - specify PyTorch type (CPU vs GPU) for each group, but can also install all groups at once, with preference for whatever is defined in `[tool.poetry.extras]`. Since I use all the groups for development, but separate groups for building Docker containers.

---

_Comment by @charliermarsh on 2025-06-28 13:09_

Semantically, though, it isn't possible to "install all groups at once" if the groups include conflicting versions of PyTorch. Like, you cannot satisfy both the `train_model` and  `inference` groups at once -- if you install the GPU version, then `inference` is actually _not_ satisfied, you're using some other mix of your dependencies.

---

_Comment by @j-adamczyk on 2025-06-28 13:26_

Yes, that is technically true, I admit. Still, I know about those conflicts, and I explicitly want to select one version over the other in case of installing all groups, like I do in Poetry.

---
