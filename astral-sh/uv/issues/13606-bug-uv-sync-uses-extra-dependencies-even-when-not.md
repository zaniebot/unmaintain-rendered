---
number: 13606
title: "Bug? `uv sync` uses extra dependencies even when not requested"
type: issue
state: closed
author: Guitaricet
labels:
  - question
assignees: []
created_at: 2025-05-22T23:29:12Z
updated_at: 2025-05-24T12:19:44Z
url: https://github.com/astral-sh/uv/issues/13606
synced_at: 2026-01-10T01:25:35Z
---

# Bug? `uv sync` uses extra dependencies even when not requested

---

_Issue opened by @Guitaricet on 2025-05-22 23:29_

### Summary

Following [the tutorial for no-build-isolation](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) dependencies but replacing `flash-attn` with a package provided via github link seems to break uv sync.

Specifically, even though I do not provide `--extra compile` it seems to try to install compile-time dependency (tried with both no `--extra` and with `--extra build`)

Am I missing something in the configuration or is it a uv bug?

```
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
build = ["torch", "setuptools", "packaging"]
compile = ["mmpose"]

[tool.uv]
no-build-isolation-package = ["mmpose"]

[[tool.uv.dependency-metadata]]
name = "mmpose"
requires-dist = ["torch", "setuptools"]

[tool.uv.sources]
mmpose = { git = "https://github.com/ViTAE-Transformer/ViTPose.git", rev = "d5216452796c90c6bc29f5c5ec0bdba94366768a" }
```

Error:
```
vlad@bfgpu2 ~/w/a/a/not_giga_track ❯❯❯ uv sync --extra build --no-cache --verbose
DEBUG uv 0.7.7
DEBUG Found project root: `/home/vlad/worktrees/add_hamer/apps/not_giga_track`
DEBUG Project is contained in non-workspace project: `/home/vlad/worktrees/add_hamer/apps`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/vlad/worktrees/add_hamer/apps/not_giga_track`
DEBUG No Python version file found in workspace: /home/vlad/worktrees/add_hamer/apps/not_giga_track
DEBUG Using Python request `>=3.10` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.10`
DEBUG Released lock at `/tmp/uv-8a3054bb6e5225fb.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: project @ file:///home/vlad/worktrees/add_hamer/apps/not_giga_track
DEBUG Project is contained in non-workspace project: `/home/vlad/worktrees/add_hamer/apps`
DEBUG No workspace root found, using project root
WARN No version found in dependency metadata entry for `mmpose`
DEBUG Attempting GitHub fast path for: mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose.git@d5216452796c90c6bc29f5c5ec0bdba94366768a
DEBUG Querying GitHub for commit at: https://api.github.com/repos/ViTAE-Transformer/ViTPose/commits/d5216452796c90c6bc29f5c5ec0bdba94366768a
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/ViTAE-Transformer/ViTPose/d5216452796c90c6bc29f5c5ec0bdba94366768a/pyproject.toml
DEBUG Checking netrc for credentials for https://raw.githubusercontent.com/ViTAE-Transformer/ViTPose/d5216452796c90c6bc29f5c5ec0bdba94366768a/pyproject.toml
DEBUG GitHub API returned a 404 for: https://raw.githubusercontent.com/ViTAE-Transformer/ViTPose/d5216452796c90c6bc29f5c5ec0bdba94366768a/pyproject.toml
DEBUG Fetching source distribution from Git: https://github.com/ViTAE-Transformer/ViTPose.git
DEBUG Acquired lock for `https://github.com/vitae-transformer/vitpose`
DEBUG Updating Git source `https://github.com/ViTAE-Transformer/ViTPose.git`
   Updating https://github.com/ViTAE-Transformer/ViTPose.git (d5216452796c90c6bc29f5c5ec0bdba94366768a)
DEBUG Skipping GitHub fast path; full commit hash provided: d5216452796c90c6bc29f5c5ec0bdba94366768a
DEBUG Performing a Git fetch for: https://github.com/ViTAE-Transformer/ViTPose.git
DEBUG Reset /tmp/.tmpU1NP9H/git-v0/checkouts/a4bbcc9f2738db1d/d521645 to d5216452796c90c6bc29f5c5ec0bdba94366768a
    Updated https://github.com/ViTAE-Transformer/ViTPose.git (d5216452796c90c6bc29f5c5ec0bdba94366768a)
DEBUG Released lock at `/tmp/.tmpU1NP9H/git-v0/locks/a4bbcc9f2738db1d`
DEBUG Acquired lock for `/tmp/.tmpU1NP9H/sdists-v9/git/3db644b052beafe6/d5216452796c90c6`
DEBUG No `pyproject.toml` available for: mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose.git@d5216452796c90c6bc29f5c5ec0bdba94366768a
DEBUG No static `PKG-INFO` available for: mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose.git@d5216452796c90c6bc29f5c5ec0bdba94366768a (MissingPkgInfo)
DEBUG Preparing metadata for: mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose.git@d5216452796c90c6bc29f5c5ec0bdba94366768a
DEBUG Proceeding without build isolation
DEBUG Calling `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel()`
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 8, in <module>
DEBUG     from setuptools.build_meta import __legacy__ as backend
DEBUG ModuleNotFoundError: No module named 'setuptools'
DEBUG Released lock at `/tmp/.tmpU1NP9H/sdists-v9/git/3db644b052beafe6/d5216452796c90c6/.lock`
  × Failed to build `mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose.git@d5216452796c90c6bc29f5c5ec0bdba94366768a`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
          from setuptools.build_meta import __legacy__ as backend
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build environment.
```

### Platform

Ubuntu 22.04

### Version

0.7.7

### Python version

3.10, 3.12

---

_Label `bug` added by @Guitaricet on 2025-05-22 23:29_

---

_Comment by @charliermarsh on 2025-05-22 23:48_

What you're seeing there is actually a failure to resolve the _dependencies_ for `mmpose`. We still need to resolve the dependencies for all extras, since we create a universal lockfile for all the dependencies in the project.

---

_Comment by @charliermarsh on 2025-05-22 23:50_

If you want to avoid resolving, you should be able to get this working with:

```toml
[[tool.uv.dependency-metadata]]
name = "ViTPose"
version = "0.24.0"
requires-dist = [
    "chumpy",
    "dataclasses; python_version == '3.6'",
    "json_tricks",
    "matplotlib",
    "munkres",
    "numpy",
    "opencv-python",
    "pillow",
    "scipy",
    "torchvision",
    "xtcocotools>=1.8",
]
```

That tells uv what metadata to expect, so it can skip the build. (I didn't test this, sorry!)

---

_Label `bug` removed by @konstin on 2025-05-23 08:16_

---

_Label `question` added by @konstin on 2025-05-23 08:16_

---

_Closed by @charliermarsh on 2025-05-24 12:19_

---
