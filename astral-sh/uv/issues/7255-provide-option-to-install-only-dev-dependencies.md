---
number: 7255
title: Provide option to install only dev dependencies
type: issue
state: closed
author: paveldedik
labels:
  - enhancement
assignees: []
created_at: 2024-09-10T14:10:53Z
updated_at: 2024-09-16T20:06:22Z
url: https://github.com/astral-sh/uv/issues/7255
synced_at: 2026-01-10T01:24:12Z
---

# Provide option to install only dev dependencies

---

_Issue opened by @paveldedik on 2024-09-10 14:10_

Hi,

Thank you for creating this amazing tool! We are in the process of switching from `pip-tools` to `uv` and have encountered a situation we’re unsure how to resolve.

## Use Case

We have a Python project with the following setup:

1. We install dependencies (excluding dev dependencies) in Docker.
2. We use Docker Compose to mount the `site-packages` directory to the host machine.
3. On the host machine, we create a virtual environment (`venv`).
4. Dev dependencies are installed only in this virtual environment.
5. We link the production dependencies (mounted via Docker) to the `venv` using the following script:

   ```shell
   source .venv/bin/activate
   VENV_SITE_PACKAGES=$(python -c "import site; print(site.getsitepackages()[0])")
   echo "$(realpath var/site-packages/)" > "$VENV_SITE_PACKAGES/core.pth"
   echo "$(realpath app/libs/)" > "$VENV_SITE_PACKAGES/libs.pth"
   ```

This setup works well with `pip` and `pip-compile`, allowing us to maintain a development environment close to production, with full IDE compatibility.

## Challenge with uv

We’d like to transition to `pyproject.toml` + `uv.lock` for dependency management.

Steps 1-3 remain the same when using `uv`. However, we encounter difficulties with step 4, where we need to install only dev dependencies into the virtual environment.

The issue is that:

- `uv sync` installs both production and dev dependencies.
- `uv pip install` doesn't respect the pinned dependencies from the lock file.

We’ve considered using the following workaround:

- `uv export --format requirements-txt` followed by `uv pip install <exported-requirements>`, but there doesn’t seem to be an option to export only dev dependencies as well (this could be resolved somehow by combination of `uv export ...` - `uv export --no-dev ...` outputs, but that is very far from a neat solution.

## Conclusion

There may be a better way to handle this setup with `uv` where project dependencies are mounted over Docker (feel free to suggest them), but at the moment we are missing a way to install only dev dependencies without affecting the linked site-packages.

The obvious solution is to add an option something like `uv sync --only-dev` + also `uv export --only-dev ...`.

Any guidance or suggestions would be greatly appreciated!

---

_Comment by @charliermarsh on 2024-09-10 17:47_

Agree, I do want to support this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-12 01:53_

---

_Label `enhancement` added by @charliermarsh on 2024-09-12 01:53_

---

_Referenced in [astral-sh/uv#7367](../../astral-sh/uv/pulls/7367.md) on 2024-09-13 16:21_

---

_Closed by @charliermarsh on 2024-09-16 20:06_

---

_Closed by @charliermarsh on 2024-09-16 20:06_

---

_Referenced in [astral-sh/uv#7993](../../astral-sh/uv/issues/7993.md) on 2024-10-08 02:25_

---
