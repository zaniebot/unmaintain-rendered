```yaml
number: 12467
title: egg-info directory keep being created
type: issue
state: closed
author: clement-escolano
labels:
  - question
assignees: []
created_at: 2025-03-25T15:23:37Z
updated_at: 2025-12-25T09:40:39Z
url: https://github.com/astral-sh/uv/issues/12467
synced_at: 2026-01-10T03:11:33Z
```

# egg-info directory keep being created

---

_Issue opened by @clement-escolano on 2025-03-25 15:23_

### Question

Hello

I recently migrated my project to uv (fantastic work, thank you) but there is one thing that is bothering me. From time to time (every time I run `uv add ...` at least), a directory `my_project.egg-info` is created inside my project.

I think uv tries to build my project as a library but this is not a library and I would like to prevent this behaviour (this does not happens on another small project so there may be something specific in this project).

I tried adding the following setting but it did not change anything:

```toml
[tool.uv]
package = false
```

Do you know why it can happen and how to prevent it?

Thank you

### Platform

macOS 15 arm64

### Version

uv 0.6.3 (Homebrew 2025-02-24)

---

_Label `question` added by @clement-escolano on 2025-03-25 15:23_

---

_Comment by @zanieb on 2025-03-25 17:27_

Can you share the full `pyproject.toml`? This just happens on `uv add`?

---

_Comment by @clement-escolano on 2025-03-25 20:06_

Thank you for your message.

Here is a simple file that reproduces the issue in my project:

<details>

<summary>pyproject.toml file</summary>

```toml
[project]
name = "my-project"
dynamic = ["version"]
requires-python = ">=3.12,<3.13"
dependencies = [
    # ...
]

[dependency-groups]
dev = [
    # ...
]

[tool.uv]
package = false

[tool.setuptools]
py-modules = []
```

</details>

I tried some commands, it created the directory when using `uv add`, `uv remove` and `uv build`.

Probably the root cause is that the setting `py-modules = []` is required in order for the commandes to succeed. If this setting is remove, there is the following error:

<details>

<summary>Error content</summary>

```
error: Failed to generate package metadata for `my-project @ virtual+.`
  Caused by: The build backend returned an error
  Caused by: Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

[stderr]
error: Multiple top-level packages discovered in a flat-layout: [<REDACTED>].

To avoid accidental inclusion of unwanted files or directories,
setuptools will not proceed with this build.

If you are trying to create a single distribution with multiple packages
on purpose, you should not rely on automatic discovery.
Instead, consider the following options:

1. set up custom discovery (`find` directive with `include` or `exclude`)
2. use a `src-layout`
3. explicitly set `py_modules` or `packages` with a list of names

To find more information, look for "package discovery" on setuptools docs.

hint: This usually indicates a problem with the package or the build environment.
```

</details>

---

_Comment by @charliermarsh on 2025-03-26 01:17_

It's because you have `dynamic = ["version"]`. We have to get the version _somehow_, and the way to do that (per the standards) is to ask the build backend. Because you don't have a backend defined, we fall back to `setuptools` (per the standards), which then creates the `.egg-info` directory.

---

_Comment by @clement-escolano on 2025-03-26 08:15_

OK thank you for the clarification!
I will set a fixed version then :-)

---

_Closed by @clement-escolano on 2025-03-26 08:15_

---

_Comment by @waketzheng on 2025-12-25 09:21_

This worked for me:
```TOML
[project]
name = "myproject"
dynamic = ["version"]
description = ""
authors = []
requires-python = ">=3.12"
readme = "README.md"
license = {text = "MIT"}
dependencies = [
    "kivy[base]>=2.3.1",
]

[dependency-groups]
dev = [
    "buildozer (>=1.5.0,<2.0.0)",
    "cython (>=0.29,<0.30)",
    "mypy>=1.19.1",
    "setuptools>=80.9.0",
]

[tool.mypy]
pretty = true
ignore_missing_imports = true
check_untyped_defs = true
warn_unused_ignores = true
exclude = ['.*\.bak', "bin/"]

[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"

[tool.pdm]
distribution = false
version = {source="file", path="main.py"}

[tool.uv]
package = false

[tool.uv.sources]
buildozer = { git = "ssh://git@github.com/kivy/buildozer" }

[tool.setuptools]
py-modules = []
```

---
