---
number: 6472
title: "Add `uv sync --dev-only` or equivalent"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-08-22T23:24:20Z
updated_at: 2024-09-16T20:06:22Z
url: https://github.com/astral-sh/uv/issues/6472
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `uv sync --dev-only` or equivalent

---

_Issue opened by @charliermarsh on 2024-08-22 23:24_

This was a feature request, it seems reasonable to me.

---

_Label `enhancement` added by @charliermarsh on 2024-08-22 23:24_

---

_Label `cli` added by @charliermarsh on 2024-08-22 23:24_

---

_Comment by @zanieb on 2024-08-22 23:48_

Including the root project, I presume? `--only-dev` for consistency with `--with` and `--no-install`?

---

_Comment by @charliermarsh on 2024-08-22 23:52_

Yeah I prefer `--only-dev`. I was thinking exclusive of the root project but... I'm not sure?

---

_Comment by @uptickmetachu on 2024-08-24 07:37_

We also need the ability to `uv pip install pyproject.toml --dev` (including dev requirements). 

Necessary to install dev requirements into a docker containers `--system`. 

---

_Comment by @charliermarsh on 2024-08-28 23:30_

I wonder if this should instead be `--prod` and `--no-prod`, like `--dev` and `--no-dev`.

---

_Comment by @zanieb on 2024-08-29 03:24_

I feel like `--prod` might imply something more, like ignoring "sources"?

---

_Referenced in [astral-sh/uv#7245](../../astral-sh/uv/issues/7245.md) on 2024-09-10 13:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-15 23:39_

---

_Referenced in [astral-sh/uv#7367](../../astral-sh/uv/pulls/7367.md) on 2024-09-15 23:39_

---

_Closed by @charliermarsh on 2024-09-16 20:06_

---

_Closed by @charliermarsh on 2024-09-16 20:06_

---
