```yaml
number: 16471
title: Support local package-specific index overrides without modifying pyproject.toml
type: issue
state: open
author: patrontheo
labels:
  - question
assignees: []
created_at: 2025-10-27T11:57:48Z
updated_at: 2025-10-27T18:12:22Z
url: https://github.com/astral-sh/uv/issues/16471
synced_at: 2026-01-12T16:02:32Z
```

# Support local package-specific index overrides without modifying pyproject.toml

---

_@patrontheo_

### Question

**Description / Problem:**
I’m developing a library that depends on torch. For local development, I want to use a specific CUDA version (e.g., CUDA 11.8), but I also want to keep the pyproject.toml clean and generic so downstream projects can choose their own CUDA version.

Currently, uv only allows package-specific sources via [tool.uv.sources] in the main pyproject.toml. There is no documented or working way to specify a package-specific index for local development without modifying the main pyproject.toml.

What I tried:

**1. Specify sources and index in my library’s pyproject.toml**

```toml
[sources]
torch = [
  { index = "pytorch-cu118", marker = "platform_system == 'Linux'" },
]
torchvision = [
  { index = "pytorch-cu118", marker = "platform_system == 'Linux'" },
]

[index]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
```
Result: installing my library in another project with torch+cu128 failed:
```perl
× Failed to resolve dependencies for `my-library` (v0.1.0)
  ╰─▶ Requirements contain conflicting indexes for package `torch` in split `sys_platform == 'linux'`:
      - https://download.pytorch.org/whl/cu118
      - https://download.pytorch.org/whl/cu128
```
I thought [tool.uv.sources] and index weren’t supposed to be read by consumers but maybe I'm wrong here. 
**In any case, I think 'my-library' pyproject file should be clear of any cuda dependency (to leave it free for downstream projects), so I don't think I should be defining any sources or indexes here.**

**2. Use UV_EXTRA_INDEX_URL**
```bash
UV_EXTRA_INDEX_URL=https://download.pytorch.org/whl/cu118 uv sync
```
Works as a fallback, but doesn’t guarantee the desired CUDA version if torch exists on PyPI (it installs the CPU or another CUDA variant first). So when doing local development on my-library, I can't hardcode the cuda version.

**3. Use uv.toml or a local pyproject.local.toml with [tool.uv.sources]**

Fails: uv rejects [tool.uv.sources] outside the main pyproject.toml.

Goal / Proposed feature:

- Allow a local, non-committed package-specific index override (e.g., for torch)
- Applies only to local development, leaving pyproject.toml clean
- Works even if consumers install the library with a different CUDA version

Motivation / Use case:
- Libraries depending on PyTorch (or other packages with multiple binary variants) often need a specific CUDA version during development.
- Downstream projects may require a different CUDA version.
- Right now, there is no clean way to configure this in uv without modifying pyproject.toml or globally replacing indexes.

### Platform

all

### Version

uv 0.9.5

---

_Label `question` added by @patrontheo on 2025-10-27 11:57_

---

_Comment by @patrontheo on 2025-10-27 16:02_

I realize that it's kind of a duplicate of #6772.

---

_Comment by @konstin on 2025-10-27 16:40_

`[tool.uv.sources]` aren't used when installing from a source distribution or wheel. How do your users install package, do use the package published to an index, or do they use a Git dependency with uv?

You can also use `[dependency-groups]` and sources that apply to a group for development, as `[dependency-groups]` are never used downstream.

---

_Comment by @patrontheo on 2025-10-27 16:59_

@konstin thanks for the answer.

The users install the package with `uv` and a pyproject file as follows:
```toml
dependencies = [
    "torch==2.5.1",
    "my-library"
]

[tool.uv.sources]
my-library = { git = "https://github.com/company/name-of-repo.git", subdirectory = "my-library-subdir" }
torch = [
  { index = "pytorch-cu128", marker = "platform_system == 'Linux'" },
]

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

If the pyproject of `my-library` does **not** contain [sources] nor [index] (like below), I can successfully install the library with the pyproject shown above (running `uv sync`):
```toml
dependencies = [
  "torch>=2.5",
]
```

If the pyproject of `my-library` contains [sources] and [index] (like below), it fails to install with the error I mentioned in the issue description.
```toml
dependencies = [
  "torch>=2.5",
]

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = false

[tool.uv.sources]
torch = [
  { index = "pytorch-cu118", marker = "platform_system == 'Linux'" },
]
```

Are you saying that it shouldn't behave like that and that it's a bug ?

What I would like is to be able to specify the source for `torch` when developing `my-library`, without this source being used when installing this lib as a dependency.

Maybe I am doing something wrong here ? I am not sure how `[dependency-groups]` can be used here ? Can you explain a bit more about that ?

---

_Comment by @konstin on 2025-10-27 18:01_

I think the ideal solution is being able to deactivate sources, especially index sources, for packages you depend on:

```toml
[tool.uv.sources]
my-library = { git = "https://github.com/company/name-of-repo.git", subdirectory = "my-library-subdir", sources = false }
torch = [
  { index = "pytorch-cu128", marker = "platform_system == 'Linux'" },
]
```

I thought we had this tracked somewhere, but I can't find the issue. https://github.com/astral-sh/uv/issues/6772 is also an option for this too, though deactivating sources fits better into uv's design.

The dependency groups approach doesn't work with this setup specifically.

---

_Comment by @patrontheo on 2025-10-27 18:12_

Yes, this would work indeed :)

Although from my point of view, it seems to be better that the user can install the package without having to do anything special about it (e.g. without having to put `sources = false`).

He just specifies the package as a dependency, and if he needs he adds sources / indexes for a specific dependency he has (e.g. Pytorch).

Then it means that the library pyproject should be clear of any sources, and those could be defined in a `uv.toml` file (or e.g. `pyproject.dev.toml`) if needed when developing this lib.

At least I see that as a better solution for packages like pytorch, maybe for some other types of packages the `sources = false` approach works better.

---
