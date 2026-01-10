---
number: 12233
title: Use build backend from venv (hatchling)
type: issue
state: closed
author: pycaw
labels:
  - question
assignees: []
created_at: 2025-03-17T09:17:57Z
updated_at: 2025-03-17T14:14:48Z
url: https://github.com/astral-sh/uv/issues/12233
synced_at: 2026-01-10T01:25:17Z
---

# Use build backend from venv (hatchling)

---

_Issue opened by @pycaw on 2025-03-17 09:17_

### Question

How can I tell uv to use hatchling that I installed to the venv (uv add hatchling --group dev)?

It uses `~/.cache/uv/builds-v0/.tmpkMKgQV/lib/python3.10/site-packages/hatchling` no matter what. I'd want this so that the hook I develop as part of the projects gets lintiing from the same version of hatchling that I build the project with. Or would this would be kind of an antipattern?

### Platform

linux x86_64

### Version

uv 0.6.2

---

_Label `question` added by @pycaw on 2025-03-17 09:17_

---

_Comment by @charliermarsh on 2025-03-17 13:04_

I think you could add:

```toml
[tool.uv.sources]
hatchling = { path = "./path/to/hatchling" }
```

To the `pyproject.toml` of the project that's being built (i.e., the one that has `hatchling` in its `[build-system]`).

---

_Comment by @pycaw on 2025-03-17 13:34_

> I think you could add:
> 
> [tool.uv.sources]
> hatchling = { path = "./path/to/hatchling" }
> 
> To the `pyproject.toml` of the project that's being built (i.e., the one that has `hatchling` in its `[build-system]`).

I don't see this working as,
> [Path](https://docs.astral.sh/uv/concepts/projects/dependencies/#path): A local wheel, source distribution, or project directory.

Point to hatchling sitting in a venv's site-packages dir won't satisfy this.

---

_Comment by @charliermarsh on 2025-03-17 13:36_

In a venv's site-packages dir? That would definitely be an anit-pattern.

---

_Comment by @pycaw on 2025-03-17 13:44_

So answer is then, it seems, no, we can't use hatchling that is in the project's venv or that is there as a uv tool.

---

_Comment by @charliermarsh on 2025-03-17 13:46_

No, because that's not a "Python package". It's just a Python module.

---

_Comment by @charliermarsh on 2025-03-17 13:46_

Given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "Charlie Marsh", email = "charlie.r.marsh@gmail.com" }
]
requires-python = ">=3.13.0"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
hatchling = { path = "../.venv/lib/python3.13/site-packages/hatchling"}
```

You get:

```
❯ uv build
Building source distribution...
  × Failed to build `/Users/crmarsh/workspace/uv/foo`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `hatchling @ file:///Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling`
  ├─▶ Failed to build `hatchling @ file:///Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling`
  ╰─▶ /Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

---

_Closed by @charliermarsh on 2025-03-17 13:46_

---

_Comment by @pycaw on 2025-03-17 14:06_

> Given:
> 
> [project]
> name = "foo"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> authors = [
>     { name = "Charlie Marsh", email = "charlie.r.marsh@gmail.com" }
> ]
> requires-python = ">=3.13.0"
> dependencies = []
> 
> [build-system]
> requires = ["hatchling"]
> build-backend = "hatchling.build"
> 
> [tool.uv.sources]
> hatchling = { path = "../.venv/lib/python3.13/site-packages/hatchling"}
> 
> You get:
> 
> ```
> ❯ uv build
> Building source distribution...
>   × Failed to build `/Users/crmarsh/workspace/uv/foo`
>   ├─▶ Failed to resolve requirements from `build-system.requires`
>   ├─▶ No solution found when resolving: `hatchling @ file:///Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling`
>   ├─▶ Failed to build `hatchling @ file:///Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling`
>   ╰─▶ /Users/crmarsh/workspace/uv/.venv/lib/python3.13/site-packages/hatchling does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
> ```

Yes, that's been my experience as well.

---

_Comment by @charliermarsh on 2025-03-17 14:07_

What problem are you trying to solve? Maybe I can help you find another way to get to the same outcome.

---

_Comment by @pycaw on 2025-03-17 14:14_

Originally this:
> ... so that the hook I develop as part of the project gets lintiing from the same version of hatchling that I build the project with

I wanted to avoid using hatchling sourced from different paths and instead have a single one for these two purposes.

Since then I simply installed it in my venv and created the hook aided by the sweet linter handholding.

---
