---
number: 10678
title: "Using multiple lock files / support for files not called `uv.lock`"
type: issue
state: closed
author: dionhaefner
labels: []
assignees: []
created_at: 2025-01-16T13:46:53Z
updated_at: 2025-01-16T17:57:21Z
url: https://github.com/astral-sh/uv/issues/10678
synced_at: 2026-01-07T13:12:18-06:00
---

# Using multiple lock files / support for files not called `uv.lock`

---

_Issue opened by @dionhaefner on 2025-01-16 13:46_

I want to keep a lock file that represents a reproducible single-source truth for a tested and validated environment, so we can use it during deployment. For this, we have several CI jobs that use it in tests and publish (via `uv sync --locked`), with a cron job that updates the environment periodically (via `uv lock --upgrade`).

On top of this, we have general dev workflows using uv that may lead to changes in the lock file, which is undesired.

Ideally, we'd have a lock file called `production.lock` and one regular `uv.lock` (which may or may not be gitignored) for dev work. Is there support for this use case? I couldn't find an option to specify a different name for the lock file.

---

_Comment by @charliermarsh on 2025-01-16 15:02_

No, this isn't supported.

---

_Closed by @zanieb on 2025-01-16 17:56_

---

_Comment by @zanieb on 2025-01-16 17:57_

I think this is roughly a duplicate of https://github.com/astral-sh/uv/issues/6830

---
