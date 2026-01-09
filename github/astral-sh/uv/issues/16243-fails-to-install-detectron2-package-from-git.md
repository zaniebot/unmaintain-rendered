---
number: 16243
title: Fails to install detectron2 package from git despite no-build-isolation-package set
type: issue
state: closed
author: 123marble
labels:
  - error messages
assignees: []
created_at: 2025-10-10T22:48:33Z
updated_at: 2025-10-29T00:22:26Z
url: https://github.com/astral-sh/uv/issues/16243
synced_at: 2026-01-07T13:12:19-06:00
---

# Fails to install detectron2 package from git despite no-build-isolation-package set

---

_Issue opened by @123marble on 2025-10-10 22:48_

### Summary

```
$ uv sync
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
  × Failed to build `detectron2 @ git+https://github.com/facebookresearch/detectron2.git`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'setuptools'

      hint: This error likely indicates that `detectron2` depends on `setuptools`, but doesn't declare it as a build dependency. If `detectron2`
      is a first-party package, consider adding `setuptools` to its `build-system.requires`. Otherwise, either add it to your `pyproject.toml`
      under:

      [tool.uv.extra-build-dependencies]
      detectron2 = ["setuptools"]

      or `uv pip install setuptools` into the environment and re-run with `--no-build-isolation`.
```

pyproject.toml
```
[project]
name = "test"
version = "0.1.0"
requires-python = ">=3.9"

dependencies = [
    "torch",
    "torchvision",
    "detectron2 @ git+https://github.com/facebookresearch/detectron2.git",
]

[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"

[[tool.uv.index]]
name = "pypi"
url  = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "pytorch-cu128"
url  = "https://download.pytorch.org/whl/cu128"

[tool.uv]
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
torch = { index = "pytorch-cu128" }
torchvision = { index = "pytorch-cu128" }

[tool.uv.extra-build-dependencies]
detectron2 = [
  "setuptools"
]
```

UV cannot find setuptools even with no-build-isolation set and setuptools in extra-build-dependencies

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.8.22

### Python version

_No response_

---

_Label `bug` added by @123marble on 2025-10-10 22:48_

---

_Renamed from "Fails to install detectron2 package from git despite no-build-isolation-package set." to "Fails to install detectron2 package from git despite no-build-isolation-package set" by @123marble on 2025-10-10 22:49_

---

_Comment by @konstin on 2025-10-22 12:19_

Deactivating build isolation for a package makes it ignore extra build dependencies. The following configuration without the `no-build-isolation-package` works:

```
[tool.uv.extra-build-dependencies]
detectron2 = [
  "setuptools",
  "torch"
]
```

We should add a warning when using both for the same package.

---

_Label `bug` removed by @konstin on 2025-10-22 12:19_

---

_Label `error messages` added by @konstin on 2025-10-22 12:19_

---

_Comment by @charliermarsh on 2025-10-29 00:22_

I'd suggest:
```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
    "torch",
    "torchvision",
    "detectron2 @ git+https://github.com/facebookresearch/detectron2.git",
]

[tool.uv.extra-build-dependencies]
detectron2 = [{ requirement = "torch", match-runtime = true }]

[[tool.uv.index]]
name = "pypi"
url  = "https://pypi.org/simple"
default = true

[[tool.uv.index]]
name = "pytorch-cu128"
url  = "https://download.pytorch.org/whl/cu128"

[tool.uv.sources]
torch = { index = "pytorch-cu128" }
torchvision = { index = "pytorch-cu128" }
```

Then you don't need to disable build isolation at all, and you'll still build against the correct PyTorch version.

---

_Closed by @charliermarsh on 2025-10-29 00:22_

---

_Referenced in [astral-sh/uv#16767](../../astral-sh/uv/issues/16767.md) on 2025-11-18 09:59_

---
