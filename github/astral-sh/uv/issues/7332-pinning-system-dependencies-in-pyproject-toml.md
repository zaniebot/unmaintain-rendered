---
number: 7332
title: Pinning system dependencies in pyproject.toml
type: issue
state: open
author: thepowerfuldeez
labels: []
assignees: []
created_at: 2024-09-12T14:14:00Z
updated_at: 2024-09-24T16:36:13Z
url: https://github.com/astral-sh/uv/issues/7332
synced_at: 2026-01-07T13:12:17-06:00
---

# Pinning system dependencies in pyproject.toml

---

_Issue opened by @thepowerfuldeez on 2024-09-12 14:14_

Hi! I have a question which is related to using NGC Docker image with pre-release torch2.5.0 included.
Version inside docker container (which comes with pre-installed CUTLASS, CUDNN and are of correct versions) is 2.5.0a0+872d972e41.nv24.08

This version is not resolvable if I add it directly to pyproject.toml in a list of dependencies. If I change to torch2.4.0 stable then all set of requirements from this torch version will be downloaded (including cuda bindings, etc). This might break compiled CUDA libraries that were included inside docker image, and increase install time.

Current approach:
1. Use docker image `nvcr.io/nvidia/pytorch:24.08-py3`
2. Set `UV_SYSTEM_PYTHON=1`
3. Install dependencies with lower level uv pip cmd: `uv pip install --system -r pyproject.toml`

Problem with that approach is that library versions are not explicitly pinned, and it's not usable in production settings. I would like to add pre-release installed torch version to pyproject.toml, let is resolve dependencies when I run `uv lock` and then use higher-level project management command `uv sync` to sync with lockfile. I could still use system python to circumvent setting up required venv, since there's conda env inside docker image with installed libraries.

Required featues:
1. Setting up pre-release and installed libraries inside system python
2. Running `uv lock --system` and `uv sync --system` to use system python and not install existing libraries (how it's done with `uv pip install --system` at the moment.


Thank you very much!

---

_Comment by @thepowerfuldeez on 2024-09-16 20:38_

@charliermarsh do you have an opinion?

---

_Comment by @thepowerfuldeez on 2024-09-24 16:33_

Hi @zanieb @konstin @charliermarsh , what do you think?

---

_Comment by @zanieb on 2024-09-24 16:36_

@thepowerfuldeez please don't ping us repeatedly. We don't have an obvious answer to this.

---

_Referenced in [bgreenawald/bananagrams#75](../../bgreenawald/bananagrams/pulls/75.md) on 2025-08-03 18:31_

---
