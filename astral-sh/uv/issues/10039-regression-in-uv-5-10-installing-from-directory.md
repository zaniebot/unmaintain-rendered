```yaml
number: 10039
title: Regression in uv 5.10 installing from directory/git package with pyproject.toml
type: issue
state: open
author: peterroelants
labels:
  - needs-mre
assignees: []
created_at: 2024-12-19T18:18:49Z
updated_at: 2025-02-17T03:07:01Z
url: https://github.com/astral-sh/uv/issues/10039
synced_at: 2026-01-12T16:00:05Z
```

# Regression in uv 5.10 installing from directory/git package with pyproject.toml

---

_@peterroelants_

`uv pip install` a directory with a `pyproject.toml` fails with uv version 0.5.10 (and works with `uv<0.5.10`)

```
❯ uv pip install <package-name> @ git+ssh://git@<package_github_url>
Using Python 3.11.11 environment at: ~/miniforge3/envs/uv_test
 Updated ssh://git@<package_github_url> (abcdefg)
  × Failed to download and build `<package-name> @
  │ git+ssh://git@<package_github_url>`
  ├─▶ Failed to parse: `/home/peter/.cache/uv/git-v0/checkouts/asduhasdf/abcdefg/pyproject.toml`
  ╰─▶ TOML parse error at line 1, column 1
        |
      1 | [project]
        | ^^^^^^^^^
      `pyproject.toml` is using the `[project]` table, but the required `project.version` field is neither set nor present in the `project.dynamic` list
```

`pyproject.toml` looks like:
```toml
[project]
name = "<package-name>"
description = "Package discription"
version = "0.0.1"
readme = "README.md"
license = {text = "Proprietary/Confidential"}
classifiers = [
    "Programming Language :: Python :: 3",
]
requires-python = ">=3.9"
dynamic = [
  "dependencies",
]

[build-system]
requires = ["setuptools>=67", "wheel", "pip"]
build-backend = "setuptools.build_meta"

[tool.setuptools.dynamic]
dependencies = {file = ["env/requirements.txt"]}

[tool.setuptools.packages.find]
where = ["./src"]

[project.urls]
source = "<package_github_url>"
```

I can successfully install the package with `uv-0.5.9` using `uv pip install <package-name> @ git+ssh://git@<package_github_url>`.

---

_Comment by @charliermarsh on 2024-12-19 18:22_

Please provide a Git repo that I can clone to reproduce. 

---

_Label `needs-mre` added by @charliermarsh on 2024-12-19 18:25_

---

_Comment by @peterroelants on 2024-12-20 08:26_

I'm sorry for not providing an MRE initially, I was having the issue with a private repo that I couldn't share.

However, I've been able to replicate it in the meantime at https://github.com/peterroelants/test_uv_pyproject_toml . The issue is a bit more complicated as I originally thought, more details are in the [README of that repo](https://github.com/peterroelants/test_uv_pyproject_toml/blob/main/README.md)

You can test it running:
```
uv pip install "test-uv-pyproject-toml @ git+ssh://git@github.com/peterroelants/test_uv_pyproject_toml.git#subdirectory=projects/subproject"
```
Using `uv=0.5.10`.

I'm not sure if it's an actual issue, but is definitely a regression (works with uv 0.5.9) and the behavior differs from pip, which also works fine. 

---

_Closed by @peterroelants on 2024-12-20 08:36_

---

_Reopened by @peterroelants on 2024-12-20 08:37_

---

_Comment by @jcreinhold on 2024-12-27 15:47_

I'm also getting this error on `uv==0.5.13` when trying to editable install a company repo with a `pyproject.toml` that uses a dynamic version. `uv==0.5.9` works fine.

---

_Comment by @jcreinhold on 2024-12-27 16:21_

My best guess is there's something incorrect with the logic [here](https://github.com/astral-sh/uv/blob/0bc33e87b8a58ae8bcff87c0ea76b37dd98324ea/crates/uv-workspace/src/pyproject.rs#L247). It's not immediately obvious what the problem is, but this was added in 0.5.10.

I can take a look later and submit a PR, FWIW.

---

_Comment by @jcreinhold on 2024-12-27 17:37_

Nevermind. The error was related to this change, but it was really because I have a `pyproject.toml` without a version at the root level (which is where the command was run) and I was installing a package in a subdirectory. It seems that the root `pyproject.toml` was parsed, even though it wasn't a dependency of the subdirectory, and that raised the error. I added a version to the root `pyproject.toml` and that solved the issue.

The error was a bit misleading. I suppose specifying the path of the problematic `pyproject.toml` in the error message would have helped me solve this problem much more quickly. Not sure if this edge case needs to be handled though.

I debugged this by setting `RUST_LOG=trace`, FWIW, @peterroelants. Perhaps that'd also be useful in your situation.

---

_Comment by @jcreinhold on 2024-12-27 17:55_

Actually @peterroelants it looks like your test project replicates exactly the issue I described above. If you put in a version into the root `pyproject.toml`, I believe you'll be able to use `uv>=0.5.10`.

I suppose I need to read more about how to actually use `uv` with a repository structured like that. In the repo causing a problem for me, the root `pyproject.toml` is for, e.g., `ruff` linting settings and some settings for the `uv` index URLs. But the repo has several completely independent projects underneath it (with their own `pyproject.toml`). The independent projects don't try to install the root repo or anything strange like that.

Is there some `uv` setting I should set in the root `pyproject.toml` to handle this situation properly?

---

_Comment by @charliermarsh on 2025-02-17 03:07_

I think the behavior here is right... If you have a `pyproject.toml` with a `[project]` table, it must have a version (or the version must be marked as dynamic). You can see the specification here: https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table. It's not uv-specific -- it's defined in the standards.

---
