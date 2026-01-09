---
number: 15078
title: uv sync and uv pip install do not share the same behaviour when working with extras
type: issue
state: closed
author: lilian-delouvy
labels:
  - question
assignees: []
created_at: 2025-08-05T08:22:57Z
updated_at: 2025-08-05T15:10:04Z
url: https://github.com/astral-sh/uv/issues/15078
synced_at: 2026-01-07T13:12:19-06:00
---

# uv sync and uv pip install do not share the same behaviour when working with extras

---

_Issue opened by @lilian-delouvy on 2025-08-05 08:22_

### Summary

Hello,

When trying to include a path dependency that can be in two different directories ('../../common_modules' locally and './common_modules' in a pipeline setting), I found out that using 'uv sync' will never work in the following pyproject.toml:

```
[project]
name = "teststep"
version = "0.1.0"
description = ""
authors = [{ name = "lilian-delouvy" }]
requires-python = ">=3.11, <3.12"
dependencies = []

[project.optional-dependencies]
local = ["common-modules"]
pipeline = ["common-modules"]

[tool.uv.sources]
common-modules = [
    { path = "../../common_modules", editable = true, extra = "local" },
    { path = "./common_modules", extra = "pipeline" }
]

[tool.uv]
package = true
conflicts = [
    [
        { extra = "local" },
        { extra = "pipeline" }
    ]
]

[tool.hatch.build.targets.sdist]
include = ["src/teststep"]

[tool.hatch.build.targets.wheel]
include = ["src/teststep"]

[tool.hatch.build.targets.wheel.sources]
"src/teststep" = "teststep"

[tool.hatch.metadata]
allow-direct-references = true

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Running ```uv sync --extra local``` here provides the following error:
```
error: Distribution not found at: file:///C:/<MY_PATH>/teststep/common_modules
```
It seems that uv still tries to resolve the "pipeline" path, which of course won't work locally.

Using ```uv pip install -r pyproject.toml --extra local``` however, will install the common_modules from '../../common_modules' without error.

I'm not sure this is the intended behaviour. From my understanding, ```uv sync --extra local``` and ```uv pip install -r pyproject.toml --extra local``` should have the same behaviour here.

This is related to [issue 15057](https://github.com/astral-sh/uv/issues/15057).

This also means that no 'uv.lock' file is required, which can be problematic to lock dependencies.

### Platform

Windows locally and ubuntu on pipeline environment

### Version

0.7.15 (4ed9c5791 2025-06-25)

### Python version

Python 3.11

---

_Label `bug` added by @lilian-delouvy on 2025-08-05 08:22_

---

_Referenced in [astral-sh/uv#15057](../../astral-sh/uv/issues/15057.md) on 2025-08-05 08:25_

---

_Comment by @charliermarsh on 2025-08-05 10:54_

This seems correct to me. `uv sync` has to create a lockfile for all environments, so those packages have to be available on the machine to create the `uv.lock`, even if they won't be _installed_ in all environments.

---

_Comment by @lilian-delouvy on 2025-08-05 12:28_

@charliermarsh  Then it seems I'm misunderstanding something. I can place the package twice at both '../../common_modules' and './common_modules' to generate the lock file, but once this is done and I'm running this in pipeline mode, where the dependency is available only on one of the two paths, which command am I supposed to run ? I tried ```uv sync --locked``` but to no avail, as uv still tries to reach common_modules on the other path

---

_Comment by @charliermarsh on 2025-08-05 12:31_

I would expect `uv sync --frozen` to work in that case (but not `--locked`, which requires both paths to ensure the lockfile is up-to-date).

---

_Label `bug` removed by @zanieb on 2025-08-05 13:00_

---

_Label `question` added by @zanieb on 2025-08-05 13:00_

---

_Comment by @lilian-delouvy on 2025-08-05 15:10_

It works. Thanks for your help and your quick answers !

---

_Closed by @lilian-delouvy on 2025-08-05 15:10_

---
