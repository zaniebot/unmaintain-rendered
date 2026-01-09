---
number: 6758
title: Document support for platform-specific dependencies in pyproject.toml
type: issue
state: closed
author: BeeGass
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-28T15:27:29Z
updated_at: 2024-11-13T02:46:45Z
url: https://github.com/astral-sh/uv/issues/6758
synced_at: 2026-01-07T13:12:17-06:00
---

# Document support for platform-specific dependencies in pyproject.toml

---

_Issue opened by @BeeGass on 2024-08-28 15:27_

## Feature Description

Currently, uv doesn't support specifying platform-specific dependencies directly in the `pyproject.toml` file. It would be incredibly useful to have the ability to define different dependencies for various platforms (e.g., Linux, macOS, Windows) within the same `pyproject.toml` file.

## Use Case

As a developer working on a project that needs to run on multiple platforms (specifically Linux and macOS with M1 chip), I need to specify different dependencies for each platform. For example, I need to install the CUDA-enabled version of JAX on Linux, but the standard version on macOS.

## Proposed Solution

Add support for platform-specific dependency sections in `pyproject.toml`. This could be implemented similar to how `pip` handles it with `sys_platform` markers, or as a new section under `[tool.uv]`. For example:

```toml
[tool.uv.platform-dependencies]
linux = [
    "jax[cuda12_pip]>=0.4.18,<0.5.0"
]
darwin = [
    "jax>=0.4.18,<0.5.0"
]
```

Or alternatively:

```toml
[project]
dependencies = [
    "common-dep1",
    "common-dep2",
    "jax>=0.4.18,<0.5.0; sys_platform == 'darwin'",
    "jax[cuda12_pip]>=0.4.18,<0.5.0; sys_platform == 'linux'"
]
```

## Benefits

1. Simplified dependency management for cross-platform projects
2. Reduced need for separate requirements files or complex setup scripts
3. Improved developer experience when working across different environments

## Additional Context

This feature would align uv more closely with other packaging tools and standards in the Python ecosystem, making it easier for developers to transition to using uv in their projects.

---

_Renamed from "title: Feature Request: Support for platform-specific dependencies in pyproject.toml" to "Feature Request: Support for platform-specific dependencies in pyproject.toml" by @BeeGass on 2024-08-28 15:27_

---

_Comment by @charliermarsh on 2024-08-28 15:29_

```toml
[project]
dependencies = [
    "common-dep1",
    "common-dep2",
    "jax>=0.4.18,<0.5.0; sys_platform == 'darwin'",
    "jax[cuda12_pip]>=0.4.18,<0.5.0; sys_platform == 'linux'"
]
```

Does this not work as-is?

---

_Label `question` added by @charliermarsh on 2024-08-28 17:23_

---

_Comment by @BeeGass on 2024-08-29 12:21_

> ```toml
> [project]
> dependencies = [
>     "common-dep1",
>     "common-dep2",
>     "jax>=0.4.18,<0.5.0; sys_platform == 'darwin'",
>     "jax[cuda12_pip]>=0.4.18,<0.5.0; sys_platform == 'linux'"
> ]
> ```
> 
> Does this not work as-is?

@charliermarsh 
This works and works really well. Cant believe I wrote this out and never actually tried this on my own. I swore I did, but anyways. Sorry for the needless feature request. Thanks so much.

Great feature by the way

---

_Comment by @charliermarsh on 2024-08-29 12:58_

No worries! Thanks for following up.

---

_Closed by @charliermarsh on 2024-08-29 12:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-29 12:58_

---

_Label `documentation` added by @zanieb on 2024-08-29 13:37_

---

_Renamed from "Feature Request: Support for platform-specific dependencies in pyproject.toml" to "Document support for platform-specific dependencies in pyproject.toml" by @zanieb on 2024-08-29 13:37_

---

_Reopened by @zanieb on 2024-08-29 13:37_

---

_Comment by @zanieb on 2024-08-29 13:38_

Mind if I keep this open? We should add some documentation around this, I've seen it a couple times.

---

_Referenced in [astral-sh/uv#7411](../../astral-sh/uv/pulls/7411.md) on 2024-09-15 17:45_

---

_Closed by @charliermarsh on 2024-09-15 21:55_

---

_Closed by @charliermarsh on 2024-09-15 21:55_

---

_Comment by @NellyWhads on 2024-11-13 02:39_

How can I replicate this for pytorch? When installing on `darwin`, I need to use the regular pypi index, however, when installing on another linux machine, I'd like to explicitly use the `https://download.pytorch.org/whl/cu118/` as per the [pytorch docs](https://pytorch.org/get-started/locally/)

Edit: never mind - found it: https://docs.astral.sh/uv/concepts/dependencies/#platform-specific-sources

---

_Referenced in [PMEAL/porespy#1046](../../PMEAL/porespy/issues/1046.md) on 2025-03-10 00:18_

---

_Referenced in [astral-sh/uv#12829](../../astral-sh/uv/issues/12829.md) on 2025-04-11 08:55_

---
