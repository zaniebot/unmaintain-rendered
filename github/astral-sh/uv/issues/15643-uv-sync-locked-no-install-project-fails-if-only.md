---
number: 15643
title: uv sync --locked --no-install-project fails if only project version changes
type: issue
state: open
author: carlosjourdan
labels:
  - bug
assignees: []
created_at: 2025-09-02T21:21:09Z
updated_at: 2025-09-03T00:34:29Z
url: https://github.com/astral-sh/uv/issues/15643
synced_at: 2026-01-07T13:12:19-06:00
---

# uv sync --locked --no-install-project fails if only project version changes

---

_Issue opened by @carlosjourdan on 2025-09-02 21:21_

### Summary

# Description

When running `uv sync --locked --no-install-project` I would expect the operation to succeed if all the dependencies are in line with the `uv.lock` file, regardless of the project version.

# Steps to reproduce

Create a new project with `uv init` and add any dependencies
Run `uv sync --no-install-project`
Run `uv sync --locked --no-install-project` and validate that it works
Update the version to `0.2.0` on the `pyproject.toml`
Run `uv sync --locked --no-install-project` and receive an error

# Context

We are developing a python library where the ci/cd pipelines auto increment the versions using `commitizen`. We also use devcontainers to standardize the development experience, but rebuilding the containers using the recommended `uv sync --locked --no-install-project` step on the dockerfile will fail if we don't resync the lockfile with every change.

### Platform

inux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.8.14

### Python version

Python 3.13.3

---

_Label `bug` added by @carlosjourdan on 2025-09-02 21:21_

---

_Comment by @charliermarsh on 2025-09-02 21:25_

Personally, I think it's correct to fail here, because the lockfile is no longer up-to-date, and the intent of `--locked` is to error if the lockfile doesn't match the project metadata. I'd probably suggest using `--frozen` here.


---

_Comment by @carlosjourdan on 2025-09-03 00:07_

Using `--frozen` would allow building an environment that doesn't match the dependencies on the `pyproject.toml`. Is there any way to generate a lockfile for the dependencies of the project, not the project itself? I thought this was kind of the point of runing `uv sync --no-install-project`.

---

_Comment by @zanieb on 2025-09-03 00:11_

Why not update the lockfile to reflect the proper project version first?

---

_Comment by @carlosjourdan on 2025-09-03 00:34_

As I mentioned, project version is incremented automatically using `commitizen`. This is post merging into a alpha/rc/main branch. Not sure it would make sense to update the lockfile at this point. I'd rather have any changes being made intentionally on PRs since they can impact the ci steps.

---
