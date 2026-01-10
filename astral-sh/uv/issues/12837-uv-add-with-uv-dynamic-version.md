---
number: 12837
title: "`uv add` with `uv-dynamic-version`?"
type: issue
state: closed
author: Ox54
labels:
  - question
assignees: []
created_at: 2025-04-11T13:43:05Z
updated_at: 2025-07-16T14:33:03Z
url: https://github.com/astral-sh/uv/issues/12837
synced_at: 2026-01-10T01:25:25Z
---

# `uv add` with `uv-dynamic-version`?

---

_Issue opened by @Ox54 on 2025-04-11 13:43_

### Question

Hi all!  I'm not entirely sure if I should be asking `uv` or the [`uv-dynamic-version`](https://github.com/ninoseki/uv-dynamic-versioning/issues/34) this question.. so I'm asking both.  Sorry in advance if this isn't the right place 😄 

I have a `uv` powered mono-repo project with many child-packages.  I'm currently attempting to integrate `uv-dynamic-version` into my project.  Things go fine, until I want to configure dynamic dependencies.

My root pyproject.toml looks like this:

```toml
[project]
name = "root-pkg"
dynamic = [
    "version",
    "dependencies",
]
description = "..."
authors = []
readme = "README.md"
requires-python = ">=3.12"

[project.optional-dependencies]
pygraphviz = [
    "pygraphviz>=1.14",
]

[build-system]
requires = [
    "hatchling",
    "uv-dynamic-versioning",
]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "uv-dynamic-versioning"

[tool.hatch.build.targets.wheel]
only-include = [
    "README.md",
]

[tool.hatch.build.targets.sdist]
include = [
    "README.md",
]

[tool.hatch.metadata.hooks.uv-dynamic-versioning]
dependencies = [
    "child-one=={{ version }}",
    "child-two=={{ version }}",
]

[tool.uv-dynamic-versioning]
vcs = "git"
style = "pep440"
bump = true

[tool.uv.sources.child-two]
workspace = true

[tool.uv.sources.child-one]
workspace = true

[tool.uv.workspace]
members = [
    "packages/child-one",
    "packages/child-two",
]

[tool.ruff]
exclude = [
    "docs/Notebooks/*.py",
]
line-length = 88

[dependency-groups]
dev = [...]
```

As you can see i've configured my dependencies to be "dynamic", and have defined them in the `tool.hatch.metadata.hooks.uv-dynamic-versioning` table. 

Now, when I try to run `uv add <pkg>`... uv tries to add the packages to `project.dependencies`, and thus fails to sync afterwards. 

> ValueError: 'dependencies' is dynamic but already listed in [project].

Is there any way to instruct uv to add dependences to `tool.hatch.metadata.hooks.uv-dynamic-versioning.dependencies` when `uv-dynamic-version` & `project.dynamic.dependencies` is configured?

If not, is this something that the `uv-dynamic-version` package would be able to accomplish?

Thanks in advance! 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Ox54 on 2025-04-11 13:43_

---

_Referenced in [ninoseki/uv-dynamic-versioning#34](../../ninoseki/uv-dynamic-versioning/issues/34.md) on 2025-04-11 23:15_

---

_Comment by @charliermarsh on 2025-07-16 14:33_

Looks like this was closed in `uv-dynamic-versioning`.

---

_Closed by @charliermarsh on 2025-07-16 14:33_

---
