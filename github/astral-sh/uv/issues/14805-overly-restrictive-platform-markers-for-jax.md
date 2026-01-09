---
number: 14805
title: "Overly restrictive platform markers for jax/jaxlib when torch is included from torch index with CPU vs GPU \"--extra\""
type: issue
state: closed
author: mscheltienne
labels:
  - bug
assignees: []
created_at: 2025-07-22T08:34:25Z
updated_at: 2025-08-06T09:26:28Z
url: https://github.com/astral-sh/uv/issues/14805
synced_at: 2026-01-07T13:12:19-06:00
---

# Overly restrictive platform markers for jax/jaxlib when torch is included from torch index with CPU vs GPU "--extra"

---

_Issue opened by @mscheltienne on 2025-07-22 08:34_

### Summary

The dependency resolution algorithm yields overly restrictive platform markers with the following `pyproject.toml`:

```
[build-system]
build-backend = 'setuptools.build_meta'
requires = ['setuptools >= 64.0.0']

[project]
dependencies = []
name = 'test_jax_resolution'
requires-python = '>=3.12'
version = '0.0.1'

[project.optional-dependencies]
cpu = [
  'jax',
  'torch>=2.7.0',
  'torchvision>=0.22.0',
]
cu128 = [
  'jax[cuda12]',
  'torch>=2.7.0',
  'torchvision>=0.22.0',
]

[tool.uv]
conflicts = [
  [
    {extra = 'cpu'},
    {extra = 'cu128'},
  ],
]
default-groups = 'all'

[[tool.uv.index]]
explicit = true
name = 'pytorch-cpu'
url = 'https://download.pytorch.org/whl/cpu'

[[tool.uv.index]]
explicit = true
name = 'pytorch-cu128'
url = 'https://download.pytorch.org/whl/cu128'

[tool.uv.sources]
torch = [
  {extra = 'cpu', index = 'pytorch-cpu'},
  {extra = 'cu128', index = 'pytorch-cu128'},
]
torchvision = [
  {extra = 'cpu', index = 'pytorch-cpu'},
  {extra = 'cu128', index = 'pytorch-cu128'},
]
```

An `uv sync --extra cpu` with the above configuration yields markers for `jaxlib`: `marker = "platform_machine == 'aarch64' and sys_platform == 'linux"` which are thus excluding the current system, thus the `uv sync --extra cpu` command yields an environment where `jax` is installed.. but `jaxlib` is missing (despite being a dependency of `jax`).

Removing the `cu128` extra, associated index and `conflicts` resolution, thus yielding the following `pyproject.toml` is surprisingly fixing the problem:

```
[build-system]
build-backend = 'setuptools.build_meta'
requires = ['setuptools >= 64.0.0']

[project]
dependencies = []
name = 'test_jax_resolution'
requires-python = '>=3.12'
version = '0.0.1'

[project.optional-dependencies]
cpu = [
  'jax',
  'torch>=2.7.0',
  'torchvision>=0.22.0',
]

[tool.uv]
default-groups = 'all'

[[tool.uv.index]]
explicit = true
name = 'pytorch-cpu'
url = 'https://download.pytorch.org/whl/cpu'

[tool.uv.sources]
torch = [
  {extra = 'cpu', index = 'pytorch-cpu'},
]
torchvision = [
  {extra = 'cpu', index = 'pytorch-cpu'},
]
```

Now, the `uv sync --extra cpu` command correctly includes `jax` and `jaxlib`, although in version `0.5.3` instead of `0.6.2`.

The removal of the `cu128` extra should not have impacted the resolution of the `cpu` extra, thus this difference seems to be a bug in the dependency resolution algorithm.

### Platform

Linux 6.14.0-24-generic x86_64 GNU/Linux

### Version

0.8.0

### Python version

Python 3.12.3

---

_Label `bug` added by @mscheltienne on 2025-07-22 08:34_

---

_Comment by @charliermarsh on 2025-07-24 13:09_

Agree that this looks like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-24 13:12_

---

_Comment by @charliermarsh on 2025-07-24 13:20_

Replacing `jax[cuda12]` with `jax` also fixes it, so something to do with the extra.

---

_Unassigned @charliermarsh by @konstin on 2025-08-03 11:24_

---

_Referenced in [astral-sh/uv#15041](../../astral-sh/uv/pulls/15041.md) on 2025-08-03 11:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-04 17:49_

---

_Assigned to @konstin by @charliermarsh on 2025-08-04 17:49_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-08-04 17:49_

---

_Closed by @konstin on 2025-08-06 09:26_

---
