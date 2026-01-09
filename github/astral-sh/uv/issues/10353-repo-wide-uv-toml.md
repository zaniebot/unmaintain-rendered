---
number: 10353
title: Repo wide uv.toml
type: issue
state: closed
author: sebastian-cradle
labels: []
assignees: []
created_at: 2025-01-07T10:06:16Z
updated_at: 2025-01-07T12:35:09Z
url: https://github.com/astral-sh/uv/issues/10353
synced_at: 2026-01-07T13:12:18-06:00
---

# Repo wide uv.toml

---

_Issue opened by @sebastian-cradle on 2025-01-07 10:06_

Hi, we have a mono repo and have multiple packages each with their own `pyproject.toml`.
There are some configuration I'd like to share between these (namely our private index, and the keyring-provider = "subprocess" setting).
Using a workspace seems not the correct  approach as they are independent packages with their own dependencies.

Is there a way to have a `uv.toml` at the repo root and when running e.g. `uv sync` in a sub-folder having it also picking that up?

This works if I copy the `uv.toml` to each sub-directory (which I'd like to avoid) or copy it to the user config (~/.config/uv), which I'd also like to avoid as these are settings only relevant for that specific repo.

From https://docs.astral.sh/uv/configuration/files/ it looked like the discovery walks up the directory structure, but these additional settings are neither picked up from the `uv.toml` in the parent nor in the `pyproject.toml` of the parent.


---

_Comment by @sebastian-cradle on 2025-01-07 12:35_

My apologies, this seems to work already!

---

_Closed by @sebastian-cradle on 2025-01-07 12:35_

---
