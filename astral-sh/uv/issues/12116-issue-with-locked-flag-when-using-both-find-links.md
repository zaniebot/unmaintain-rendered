```yaml
number: 12116
title: Issue with --locked flag when using both find-links and custom indices in workspaces
type: issue
state: closed
author: rayanramoul
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-03-11T16:33:22Z
updated_at: 2025-03-14T01:16:55Z
url: https://github.com/astral-sh/uv/issues/12116
synced_at: 2026-01-12T16:00:55Z
```

# Issue with --locked flag when using both find-links and custom indices in workspaces

---

_@rayanramoul_

First of all thanks Astral team for the amazing tool UV is!

# Summary
I've encountered what appears to be a bug when using UV's workspace feature with a specific configuration. The issue occurs when:

The root workspace `pyproject.toml` contains a `find-links` configuration
A sub-package pyproject.toml defines custom indices via `tool.uv.sources` or `tool.uv.index`

# Steps to Reproduce
Having at the root of a workspace the following `pyproject.toml`:
```toml
[project]
name = "root-project"
version = "0.0.1"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = [
  "package1"
]

[tool.uv.sources]
package1 = [
  { workspace = true },
]

[tool.uv]
find-links = [
  "https://storage.googleapis.com/libtpu-releases/index.html",
]


[tool.uv.workspace]
members = ["packages/*"]
```
And the following `package1`'s `pyproject.toml`:
```toml
[project]
name = "package1"
version = "0.1.0"
requires-python = "==3.10.*"
dependencies = [
]

[project.optional-dependencies]
cpu = [
  "torch==2.3.0",
]
gpu = [
  "torch==2.3.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cuda11", extra = "gpu" },
  { index = "pytorch-cpu", extra = "cpu" },
]


[[tool.uv.index]]
name = "pytorch-cuda11"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
conflicts = [ 
  [
    { extra = "gpu" },
    { extra = "cpu" },
  ],
]
package = true 


[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"


[tool.hatch.metadata]
allow-direct-references = true
```

With that setup running `uv sync --all-packages` (completes successfully and updates uv.lock)
And after that running
```
uv sync --all-packages --locked
uv lock --locked
```
Get you the following error:
```
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

The command should succeed since the lockfile is already updated.
It seems that the issue appears when these two configurations exist simultaneously:
In the root workspace pyproject.toml:
```toml
# root's pyproject.toml
[tool.uv]
find-links = [
  "https://storage.googleapis.com/libtpu-releases/index.html",
]
```
and 
```toml
# sub-package's pyproject.toml
[tool.uv.sources]
torch = [
  { index = "pytorch-cuda11", extra = "gpu" },
  { index = "pytorch-cpu", extra = "cpu" },
]

[[tool.uv.index]]
name = "pytorch-cuda11"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

Removing either the `find-links` from the root or the custom index from the sub-package resolves the issue.

I've created a minimal repository that reproduces this issue: [reproduction](https://github.com/rayanramoul/uv-issue-repo)

Thank you!

### Platform

MacOS

### Version

0.6.5 (Homebrew 2025-03-06)

### Python version

3.10

---

_Label `bug` added by @rayanramoul on 2025-03-11 16:33_

---

_Label `great writeup` added by @konstin on 2025-03-12 10:10_

---

_Comment by @charliermarsh on 2025-03-14 01:16_

Thanks for the clear issue. This is the same as https://github.com/astral-sh/uv/issues/11419 (though hard to tell from first glance): defining explicit indexes in child dependencies leads to consistent invalidation.

---

_Closed by @charliermarsh on 2025-03-14 01:16_

---
