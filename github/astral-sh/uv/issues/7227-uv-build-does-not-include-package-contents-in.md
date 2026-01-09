---
number: 7227
title: "`uv build` does not include package contents in wheel when the project name is different from the package"
type: issue
state: closed
author: albireox
labels: []
assignees: []
created_at: 2024-09-09T18:25:25Z
updated_at: 2024-09-16T01:57:48Z
url: https://github.com/astral-sh/uv/issues/7227
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv build` does not include package contents in wheel when the project name is different from the package

---

_Issue opened by @albireox on 2024-09-09 18:25_

I have a project with the following relevant (?) `pyproject.toml`

```toml
[project]
name = "lvm-ln2fill"
requires-python = ">=3.12,<4"

[tool.uv]
package = true

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.sdist]
packages = ["src/ln2fill"]

[tool.hatch.build.targets.wheel]
packages = ["src/ln2fill"]
```

The directory structure is

```
|
|— src/
|—— ln2fill/
|——— __init__.py
|——— ...
|— pyproject.toml
```

so that the project name (`lvm-ln2fill`) is different from the package to include `src/ln2fill`.

When I do `uv build` the source file looks fine and has all the files in `src/ln2fill` but the wheel only contains the metadata and no code files. With the same configuration, if I do `uvx hatch build` both the source and wheel look fine.

---

_Comment by @charliermarsh on 2024-09-09 18:28_

What does `python -m build` give you?

---

_Comment by @albireox on 2024-09-09 19:54_

It seems the same happens with `python -m build`. The source tar file looks fine but the wheel doesn't include any code, so maybe this is an issue with `hatch`.

---

_Comment by @albireox on 2024-09-09 20:14_

For what it's worth. This works with `setuptools` for both source and wheel.

```toml
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["src"]
include = ["ln2fill*"]
```

---

_Comment by @charliermarsh on 2024-09-16 01:57_

Okay, thanks. If you see the same behavior with `python -m build`, I consider it an issue with `hatch`, since `python -m build` is effectively a canonical build frontend. Gonna close out, but happy to answer any questions...

---

_Closed by @charliermarsh on 2024-09-16 01:57_

---
