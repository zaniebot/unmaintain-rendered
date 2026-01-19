```yaml
number: 17607
title: "uv `dependency-metadata` seems to override `dependency-groups`"
type: issue
state: open
author: KolinGuo
labels:
  - question
assignees: []
created_at: 2026-01-19T06:43:03Z
updated_at: 2026-01-19T06:44:52Z
url: https://github.com/astral-sh/uv/issues/17607
synced_at: 2026-01-19T07:23:51Z
```

# uv `dependency-metadata` seems to override `dependency-groups`

---

_@KolinGuo_

### Question

### Problem
I sometimes will encounter packages with C++/libtorch extensions that are built with `scikit-build-core` and has dynamic version metadata. These packages often requires `--no-build-isolation` to build and link against the `libtorch.so` provided by the runtime venv's torch.

This is a similar situation as https://github.com/astral-sh/uv/issues/14940, just with dynamic metadata.

From the docs, one of the recommended approach is to use `tool.uv.dependency-metadata`. However, in my tests of adding `scikit_build_core` to `dependency-groups.dev`, I found that `tool.uv.dependency-metadata` seems to override the `dependency-groups` settings. Even so, the defined `dependency-groups` are reported as not defined by `uv sync --group <group>`.

Is this the expected behavior?

### Miminal Example
```
[build-system]
requires = ["scikit-build-core"]
build-backend = "scikit_build_core.build"

[project]
name = "dummy-pkg"
dynamic = ["version"]                  # Dynamic metadata for version
description = "A dummy Python package"
requires-python = ">=3.10"
dependencies = ["torch"]

[dependency-groups]
dev = ["scikit_build_core"]

[tool.uv]
no-build-isolation-package = ["dummy-pkg"]

[[tool.uv.dependency-metadata]]
name = "dummy-pkg"
version = "0.0.1"
requires-dist = ["torch"]
```
Running `uv sync --group dev` with this `pyproject.toml` will results in
```bash
Resolved 28 packages in 0.47ms
error: Group `dev` is not defined in the project's `dependency-groups` table
```

### Similar scenario with static metadata
```
[build-system]
requires = ["scikit-build-core"]
build-backend = "scikit_build_core.build"

[project]
name = "dummy-pkg"
version = "0.0.1"
description = "A dummy Python package"
requires-python = ">=3.10"
dependencies = ["torch"]

[dependency-groups]
dev = ["scikit_build_core"]

[tool.uv]
no-build-isolation-package = ["dummy-pkg"]
```
Using this `pyproject.toml`, running `uv sync` will install `torch` and `scikit-build-core` from `dev` group first, and then successfully build and install the `dummy-pkg` in non-isolated environment. 

I expect similar config will work with dynamic metadata, but seems like I was wrong.

Thanks a lot for your help!

### Platform

Ubuntu 24.04

### Version

uv 0.9.26

---

_Label `question` added by @KolinGuo on 2026-01-19 06:43_

---
