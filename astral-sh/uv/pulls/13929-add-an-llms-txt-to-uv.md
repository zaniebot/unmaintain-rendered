```yaml
number: 13929
title: "Add an `llms.txt` to uv"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/llms
created_at: 2025-06-09T17:20:57Z
updated_at: 2025-06-25T16:57:15Z
url: https://github.com/astral-sh/uv/pull/13929
synced_at: 2026-01-12T16:10:56Z
```

# Add an `llms.txt` to uv

---

_@charliermarsh_

## Summary

The generated file looks like:

```
# uv

> uv is an extremely fast Python package and project manager, written in Rust.

You can use uv to install Python dependencies, run scripts, manage virtual environments,
build and publish packages, and even install Python itself. uv is capable of replacing
`pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.

uv includes both a pip-compatible CLI (prepend `uv` to a pip command, e.g., `uv pip install ruff`)
and a first-class project interface (e.g., `uv add ruff`) complete with lockfiles and
workspace support.


## Getting started

- [Features](https://docs.astral.sh/uv/getting-started/features/index.md)
- [First steps](https://docs.astral.sh/uv/getting-started/first-steps/index.md)
- [Installation](https://docs.astral.sh/uv/getting-started/installation/index.md)

## Guides

- [Installing Python](https://docs.astral.sh/uv/guides/install-python/index.md)
- [Publishing packages](https://docs.astral.sh/uv/guides/package/index.md)
- [Working on projects](https://docs.astral.sh/uv/guides/projects/index.md)
- [Running scripts](https://docs.astral.sh/uv/guides/scripts/index.md)
- [Using tools](https://docs.astral.sh/uv/guides/tools/index.md)

## Integrations

- [Alternative indexes](https://docs.astral.sh/uv/guides/integration/alternative-indexes/index.md)
- [AWS Lambda](https://docs.astral.sh/uv/guides/integration/aws-lambda/index.md)
- [Dependency bots](https://docs.astral.sh/uv/guides/integration/dependency-bots/index.md)
- [Docker](https://docs.astral.sh/uv/guides/integration/docker/index.md)
- [FastAPI](https://docs.astral.sh/uv/guides/integration/fastapi/index.md)
- [GitHub Actions](https://docs.astral.sh/uv/guides/integration/github/index.md)
- [GitLab CI/CD](https://docs.astral.sh/uv/guides/integration/gitlab/index.md)
- [Jupyter](https://docs.astral.sh/uv/guides/integration/jupyter/index.md)
- [marimo](https://docs.astral.sh/uv/guides/integration/marimo/index.md)
- [Pre-commit](https://docs.astral.sh/uv/guides/integration/pre-commit/index.md)
- [PyTorch](https://docs.astral.sh/uv/guides/integration/pytorch/index.md)

## Projects

- [Building distributions](https://docs.astral.sh/uv/concepts/projects/build/index.md)
- [Configuring projects](https://docs.astral.sh/uv/concepts/projects/config/index.md)
- [Managing dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/index.md)
- [Creating projects](https://docs.astral.sh/uv/concepts/projects/init/index.md)
- [Structure and files](https://docs.astral.sh/uv/concepts/projects/layout/index.md)
- [Running commands](https://docs.astral.sh/uv/concepts/projects/run/index.md)
- [Locking and syncing](https://docs.astral.sh/uv/concepts/projects/sync/index.md)
- [Using workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces/index.md)

## Features

- [Authentication](https://docs.astral.sh/uv/concepts/authentication/index.md)
- [Build backend](https://docs.astral.sh/uv/concepts/build-backend/index.md)
- [Caching](https://docs.astral.sh/uv/concepts/cache/index.md)
- [Configuration files](https://docs.astral.sh/uv/concepts/configuration-files/index.md)
- [Package indexes](https://docs.astral.sh/uv/concepts/indexes/index.md)
- [Python versions](https://docs.astral.sh/uv/concepts/python-versions/index.md)
- [Resolution](https://docs.astral.sh/uv/concepts/resolution/index.md)
- [Tools](https://docs.astral.sh/uv/concepts/tools/index.md)

## The pip interface

- [Compatibility with pip](https://docs.astral.sh/uv/pip/compatibility/index.md)
- [Locking environments](https://docs.astral.sh/uv/pip/compile/index.md)
- [Declaring dependencies](https://docs.astral.sh/uv/pip/dependencies/index.md)
- [Using environments](https://docs.astral.sh/uv/pip/environments/index.md)
- [Inspecting environments](https://docs.astral.sh/uv/pip/inspection/index.md)
- [Managing packages](https://docs.astral.sh/uv/pip/packages/index.md)

## Reference

- [Commands](https://docs.astral.sh/uv/reference/cli/index.md)
- [Environment variables](https://docs.astral.sh/uv/reference/environment/index.md)
- [Installer](https://docs.astral.sh/uv/reference/installer/index.md)
- [Settings](https://docs.astral.sh/uv/reference/settings/index.md)
```

Closes #13901.


---

_@konstin reviewed on 2025-06-10 09:05_

---

_Review comment by @konstin on `mkdocs.template.yml`:95 on 2025-06-10 09:05_

Should we be adding titles to the links? They are sometimes clearer than the filename.

---

_@charliermarsh reviewed on 2025-06-10 11:18_

---

_Review comment by @charliermarsh on `mkdocs.template.yml`:95 on 2025-06-10 11:18_

Nope, they get populated automatically. See the rendered doc in the summary.

---

_Label `documentation` added by @charliermarsh on 2025-06-10 11:46_

---

_Merged by @charliermarsh on 2025-06-10 11:46_

---

_Closed by @charliermarsh on 2025-06-10 11:46_

---

_Branch deleted on 2025-06-10 11:46_

---

_Comment by @noamta-gloat on 2025-06-25 15:38_

@charliermarsh you (or someone else on the team) should submit this to an llmstxt index such as https://llmstxt.site/.

---

_Comment by @charliermarsh on 2025-06-25 16:57_

Thanks, I submitted it.

---
