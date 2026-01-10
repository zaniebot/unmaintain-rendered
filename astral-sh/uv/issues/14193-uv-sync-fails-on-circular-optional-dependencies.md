---
number: 14193
title: uv sync fails on circular optional dependencies when installing from source
type: issue
state: closed
author: sveinse
labels:
  - question
assignees: []
created_at: 2025-06-21T22:12:53Z
updated_at: 2025-06-27T09:42:57Z
url: https://github.com/astral-sh/uv/issues/14193
synced_at: 2026-01-10T01:25:43Z
---

# uv sync fails on circular optional dependencies when installing from source

---

_Issue opened by @sveinse on 2025-06-21 22:12_

### Summary

I have an issue running `uv sync` for a project that have circular dependencies in its optional extras, and where the dependencies refer to GitHub sources. It fails with "Requirements contain conflicting URLs for package ... in all marker environments`.

What happens is that the local package `pkgb` has an optional dependency to `pkga` with GH URL. `pkga` depends on `pkgb` on GH URL.

Is there a simple way to circumvent this? Preferably in the sources and not by the installing user. Because `uv sync` is checking even on "extra-less" installs, every user installing this package from source will be hit with this issue.

### Setup

I have setup a package `pkga` which depend on `pkgb @git+https://github.com/sveinse/pkgb.git`. See repo https://github.com/sveinse/pkga/blob/main/pyproject.toml

```toml
[project]
name = "pkga"
dependencies = [
    "pkgb @git+https://github.com/sveinse/pkgb.git"
]
```

The `pkgb` repo depends __optionally__ to `pkga @git+https://github.com/sveinse/pkga.git`. See https://github.com/sveinse/pkgb/blob/main/pyproject.toml

```toml
[project]
name = "pkgb"
dependencies = []

[project.optional-dependencies]
dev = [
    "pkga @git+https://github.com/sveinse/pkga.git"
]
```

### Repro

1. `git clone https://github.com/sveinse/pkgb.git`
2. `uv sync --verbose`. See response below

```
$ uv sync --verbose
DEBUG uv 0.7.13 (62ed17b23 2025-06-12)
DEBUG Found project root: `C:\sveinse\uv-issue\pkgb`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\sveinse\uv-issue\pkgb`
DEBUG Reading Python requests from version file at `C:\sveinse\uv-issue\pkgb\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.13 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\sveinse\AppData\Roaming\uv\python`
DEBUG Python interpreter in the registry is not executable: `Software\Python\PythonCore\2.7
DEBUG Found `cpython-3.13.1-windows-x86_64-none` at `C:\Users\sveinse\AppData\Local\Programs\Python\Python313\python.exe` (registry)
Using CPython 3.13.1 interpreter at: C:\Users\sveinse\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: C:\Users\sveinse\AppData\Local\Programs\Python\Python313\python.exe
DEBUG Released lock at `C:\Users\sveinse\AppData\Local\Temp\uv-d1559711b0b131ba.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: pkgb @ file:///C:/sveinse/uv-issue/pkgb
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched extras for: `pkgb==0.1.0`
  Requested: {ExtraName("dev")}
  Existing: {}
DEBUG Attempting GitHub fast path for: pkga @ git+https://github.com/sveinse/pkga.git
DEBUG Querying GitHub for commit at: https://api.github.com/repos/sveinse/pkga/commits/HEAD
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/sveinse/pkga/650c658383d0fce2c9653fae1df40ec6c5f00bea/pyproject.toml
DEBUG Found static metadata via GitHub fast path for: pkga @ git+https://github.com/sveinse/pkga.git
DEBUG Attempting GitHub fast path for: pkgb @ git+https://github.com/sveinse/pkgb.git
DEBUG Querying GitHub for commit at: https://api.github.com/repos/sveinse/pkgb/commits/HEAD
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/sveinse/pkgb/de73b8c9901f24d9646db7237b208f85ab4a5b2a/pyproject.toml
DEBUG Found static metadata via GitHub fast path for: pkgb @ git+https://github.com/sveinse/pkgb.git
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: pkgb[dev]*
DEBUG Searching for a compatible version of pkgb @ file:///C:/sveinse/uv-issue/pkgb (*)
DEBUG Adding direct dependency: pkgb==0.1.0
DEBUG Adding direct dependency: pkgb[dev]==0.1.0
DEBUG Searching for a compatible version of pkgb @ file:///C:/sveinse/uv-issue/pkgb (==0.1.0)
DEBUG Searching for a compatible version of pkgb @ file:///C:/sveinse/uv-issue/pkgb (==0.1.0)
DEBUG Adding direct dependency: pkga*
DEBUG Searching for a compatible version of pkga @ git+https://github.com/sveinse/pkga.git (*)
DEBUG Released lock at `C:\sveinse\uv-issue\pkgb\.venv\.lock`
error: Requirements contain conflicting URLs for package `pkgb` in all marker environments:
- C:\sveinse\uv-issue\pkgb
- git+https://github.com/sveinse/pkgb.git
```

### Platform

Win 11 x86

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

### Python version

Python 3.13.1

---

_Label `bug` added by @sveinse on 2025-06-21 22:12_

---

_Comment by @konstin on 2025-06-22 13:53_

Why do you have a circular dependency, should instead one of the packages be broken into two to remove the cycle?

One option would be to turn `dev` from an extra into a dependency group, which is not used in dependency resolution.

You can use a dependency override (https://docs.astral.sh/uv/reference/settings/#override-dependencies) to force a specific URL, such as a local file URL, but it requires an absolute path for the `file:///` URL so it's somewhat suboptimal.

---

_Comment by @sveinse on 2025-06-22 19:16_

These are two packages that already exists and they used to work perfectly with pip. The optional dependency that creates the circular dependency is a little used extra option for validating the module during development.

It was a great hint to move the dependency into a dependency-group, because that's what it really is. However this do not work with uv. It still fails.
```toml
[dependency-groups]
update = [
   "pkga @git+https://github.com/sveinse/pkga.git"
]

[tool.uv]
default-groups = []
````

That being said, pip has also stated to fail on this circular reference, both as extra and as group. So I'm inclined to think that this use case is moot. Need to find another way to specify this auxiliary dependency without breaking pip or uv.

PS! I wonder, would this construct work without the `@git` references?

---

_Label `bug` removed by @konstin on 2025-06-22 19:39_

---

_Label `question` added by @konstin on 2025-06-22 19:39_

---

_Comment by @konstin on 2025-06-22 19:41_

> PS! I wonder, would this construct work without the `@git` references?

uv accepts dependency cycles, but it requires that for each package (on each platform), there is only URL, be a file path, git or https. With the git reference, it can't tell that the project you're currently developing locally refers to the same project as the git URL and errors.

---

_Comment by @sveinse on 2025-06-27 08:45_

I redid the packages by removing the `@git` dependencies, and it works fine now. So for that aspect of it, I think the issue can be closed. Not related to uv, it turns out that python packaging with the use direct URLs instead of package index is cumbersome.

---

_Closed by @konstin on 2025-06-27 09:42_

---
