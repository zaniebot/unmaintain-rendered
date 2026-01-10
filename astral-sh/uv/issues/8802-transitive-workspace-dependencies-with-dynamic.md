---
number: 8802
title: Transitive workspace dependencies with dynamic package versioning
type: issue
state: closed
author: ajohnson5
labels: []
assignees: []
created_at: 2024-11-04T09:05:04Z
updated_at: 2024-11-05T09:08:04Z
url: https://github.com/astral-sh/uv/issues/8802
synced_at: 2026-01-10T01:24:33Z
---

# Transitive workspace dependencies with dynamic package versioning

---

_Issue opened by @ajohnson5 on 2024-11-04 09:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
## UV Version

`0.4.29`

## Issue 
Currently I am experiencing an issue with transitive dependencies in workspaces when using a dynamic version. I have created a minimal example with a fork of https://github.com/konstin/workspace-git-path-dep-test. The fork is here https://github.com/ajohnson5/workspace-git-path-dep-test/tree/dynamic-version.

I am trying to import two modules (which are workspaces), one of which depends on the other, similar to packages `c` and `d`  (in the above) into another repository.

## Reproduction 
In the above fork there are two branches:

1. static-version: Same as the main branch with each package having an explicit version number defined in their pyproject.toml files such as `version="0.1.0"`.
2. dynamic-version: Each package has a dynamic version, similar to below with a `__about__.py` file in the root of the project. In this file we have `__version__=="1.0.0"`
```
[project]
name = "d"
dynamic=["version"]

[tool.hatch.version]
path = "../../__about__.py"
```


Now in a different project, I am trying to import both packages `c` and `d`. The pyproject.toml is shown below:
```
[project]
name = "example-project"
version = "0.1.0"
requires-python=">=3.11, <=3.12"

dependencies = [
  "c",
  "d",
]

[tool.uv.sources]
c = { git = "https://github.com/ajohnson5/workspace-git-path-dep-test.git", subdirectory="packages/c", branch="dynamic-version"}
d = { git = "https://github.com/ajohnson5/workspace-git-path-dep-test.git", subdirectory="packages/d", branch="dynamic-version"}


[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/example_project"]
```

Note that between any uv commands I am doing a complete fresh run, `uv cache clean`, `rm -rf .venv` and removing the uv lock.
If I use the `dynamic-version` branch I get the following error when running `uv sync` or `uv lock`

```
error: Requirements contain conflicting URLs for package `d`:
- file:///Users/a-user/.cache/uv/git-v0/checkouts/c63672d311baa9b4/d116d88/packages/d
- git+https://github.com/ajohnson5/workspace-git-path-dep-test.git@dynamic-version#subdirectory=packages/d
```

After if I change the branch to `static-version` in the above toml then the install works completely fine.


### Related Issues

https://github.com/astral-sh/uv/pull/8665


---

_Comment by @charliermarsh on 2024-11-04 13:49_

\cc @konstin 

---

_Comment by @konstin on 2024-11-05 07:13_

We currently don't support dynamic versions in workspaces. They have a number of disadvantages, e.g., we have to run python code each time we want to determine the version of a package in the workspace, instead of just reading `pyproject.toml`, and we can't see when the version changes (there's no way for uv to tell that's it's `about.py` that changes the version). Instead of defining the version in about.py and having it dynamic in pyproject.toml, I recommend setting the version in `pyproject.toml` and reading it in `about.py` with [importlib.metadata.version](https://docs.python.org/3/library/importlib.metadata.html#importlib.metadata.version).

---

_Comment by @ajohnson5 on 2024-11-05 09:08_

Makes sense, thanks for getting back to me.

---

_Closed by @ajohnson5 on 2024-11-05 09:08_

---
