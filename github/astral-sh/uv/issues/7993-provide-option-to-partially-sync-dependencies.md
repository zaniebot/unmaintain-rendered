---
number: 7993
title: "Provide option to partially sync dependencies with `uv run` and `uv sync` with specific packages"
type: issue
state: closed
author: uptickmetachu
labels:
  - projects
  - cli
assignees: []
created_at: 2024-10-08T02:25:31Z
updated_at: 2025-01-21T06:34:19Z
url: https://github.com/astral-sh/uv/issues/7993
synced_at: 2026-01-07T13:12:17-06:00
---

# Provide option to partially sync dependencies with `uv run` and `uv sync` with specific packages

---

_Issue opened by @uptickmetachu on 2024-10-08 02:25_

Hi thanks for the amazing work and velocity with UV!

This is something similar to the newly added `uv run --only-dev` and `uv sync --only-dev` closed in https://github.com/astral-sh/uv/issues/7255. 

Instead of installing **ONLY** the dev dependencies, it would be nice to specify specific packages to install / sync. 

## Use Case
Within CI we may only want to install a specific version of a tool and have it match what we have specified in `pyproject.toml`. 

For example, we may have `ruff==0.6.4` installed as a dev dependency and within a CI pipeline we want to install the same version of ruff and run it as part of the check with the command `uv run ruff` but without installing every other dependency. 

Things I've tried:
- `uv run ruff`: Currently this will sync all dependencies. 
- `uv run ruff --only-dev`: Syncs all dev dependencies which can be too heavy in some cases.
- `uv run --with ruff --no-sync ruff`: Sometimes works with a temporary ruff but sometimes picks up a completely different ruff. Ideally it should be installed using the lockfile specified version and within the projects .venv.
-  `uv run --with ruff --no-package ruff`: Same as above.





---

_Renamed from "Provide option to partially sync dependencies with `uv run` and `uv sync` with specific specified packages" to "Provide option to partially sync dependencies with `uv run` and `uv sync` with specific packages" by @uptickmetachu on 2024-10-08 02:25_

---

_Comment by @zanieb on 2024-10-08 03:52_

`--only-package <package>` seems reasonable to add as corollary to `--no-package`

---

_Label `projects` added by @zanieb on 2024-10-08 03:52_

---

_Label `cli` added by @zanieb on 2024-10-08 03:52_

---

_Comment by @Aryakoste on 2024-10-13 05:07_

Hello @zanieb I would like to work on this issue.

---

_Comment by @uptickmetachu on 2025-01-21 06:34_

Closing. My use case is now solved with the following.

`uv run --only-group GROUP_NAME --inexact COMMAND`

Thanks team!

---

_Closed by @uptickmetachu on 2025-01-21 06:34_

---
