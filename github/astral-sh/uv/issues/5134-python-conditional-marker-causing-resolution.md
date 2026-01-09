---
number: 5134
title: "Python conditional marker causing resolution issue where it shouldn't"
type: issue
state: closed
author: mkniewallner
labels:
  - resolver
  - great writeup
assignees: []
created_at: 2024-07-16T21:40:17Z
updated_at: 2024-07-17T01:19:09Z
url: https://github.com/astral-sh/uv/issues/5134
synced_at: 2026-01-07T13:12:17-06:00
---

# Python conditional marker causing resolution issue where it shouldn't

---

_Issue opened by @mkniewallner on 2024-07-16 21:40_

Stumbled upon what I think is a bug in the dependency resolution algorithm.

If a project:
- requires a minimum Python version (say `>= 3.8`)
- depends on a package that requires a higher minimum Python version than the project (say `>= 3.9`)
- conditionally requires the dependency above to match the minimum Python version of the dependency (say `>= 3.9`)

then dependency resolution fails:
```console
$ uv sync --preview
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.8) does not satisfy Python>=3.9 and foo==0.0.1 depends on pre-commit{python_version >= '3.9'}==3.7.1, we can conclude that foo==0.0.1 cannot be used.
      And because only foo==0.0.1 is available and you require foo, we can conclude that the requirements are unsatisfiable.
```

I would understand why it fails if not setting a condition for the dependency, since it's unsolvable, but because of the condition on the Python version, I would expect the resolution to work, as the dependency would simply not be installed on Python 3.8.

## Minimal reproducer

```toml
[project]
name = "foo"
version = "0.0.1"
requires-python = ">=3.8"
dependencies = ["pre-commit==3.7.1; python_version >= '3.9'"]
```

```console
$ uv sync --preview --verbose
DEBUG uv 0.2.25
DEBUG Found project root: `/home/mathieu/Workspaces/throwaways/foo`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.8 in managed installations or system path
DEBUG Searching for managed installations at `/home/mathieu/.local/share/uv/python`
DEBUG Found managed Python `cpython-3.12.1-linux-x86_64-gnu`
DEBUG Found cpython 3.12.1 at `/home/mathieu/.local/share/uv/python/cpython-3.12.1-linux-x86_64-gnu/bin/python3` (managed installations)
Using Python 3.12.1 interpreter at: /home/mathieu/.local/share/uv/python/cpython-3.12.1-linux-x86_64-gnu/bin/python3
Creating virtualenv at: .venv
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution.
DEBUG Acquired lock for `/home/mathieu/.cache/uv/built-wheels-v3/editable/74f2787c29c91621`
DEBUG Preparing metadata for: foo @ file:///home/mathieu/Workspaces/throwaways/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///home/mathieu/Workspaces/throwaways/foo (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: foo @ file:///home/mathieu/Workspaces/throwaways/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.1
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///home/mathieu/Workspaces/throwaways/foo (*)
DEBUG Adding transitive dependency for foo==0.0.1: pre-commit{python_version >= '3.9'}==3.7.1
DEBUG Found fresh response for: https://pypi.org/simple/pre-commit/
DEBUG Searching for a compatible version of pre-commit{python_version >= '3.9'} (==3.7.1)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of foo @ file:///home/mathieu/Workspaces/throwaways/foo (<0.0.1 | >0.0.1)
DEBUG No compatible version found for: foo
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.8) does not satisfy Python>=3.9 and foo==0.0.1 depends on pre-commit{python_version >= '3.9'}==3.7.1, we can conclude that foo==0.0.1 cannot be used.
      And because only foo==0.0.1 is available and you require foo, we can conclude that the requirements are unsatisfiable.
```

You can see the minimum required Python version of the version of `pre-commit` used in the reproducer [here](https://github.com/pre-commit/pre-commit/blob/v3.7.1/setup.cfg#L27).

---

_Comment by @zanieb on 2024-07-16 21:47_

Thanks for the clear report! We have a lot of work in-flight in this area, someone should be able to weigh in with some details soon. cc @konstin / @BurntSushi 

---

_Label `great writeup` added by @zanieb on 2024-07-16 21:47_

---

_Label `resolver` added by @zanieb on 2024-07-16 21:47_

---

_Comment by @charliermarsh on 2024-07-17 01:18_

I believe this is the same as https://github.com/astral-sh/uv/issues/4669. Gonna merge into that issue.

---

_Comment by @charliermarsh on 2024-07-17 01:19_

Or, more specifically, it's the same as https://github.com/astral-sh/uv/issues/4668 (which is slightly different).

---

_Closed by @charliermarsh on 2024-07-17 01:19_

---
