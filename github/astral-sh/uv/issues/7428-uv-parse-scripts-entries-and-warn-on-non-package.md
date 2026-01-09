---
number: 7428
title: "uv: parse scripts entries and warn on non-package projects"
type: issue
state: open
author: lucab
labels:
  - cli
assignees: []
created_at: 2024-09-16T14:22:08Z
updated_at: 2024-09-16T14:23:35Z
url: https://github.com/astral-sh/uv/issues/7428
synced_at: 2026-01-07T13:12:17-06:00
---

# uv: parse scripts entries and warn on non-package projects

---

_Issue opened by @lucab on 2024-09-16 14:22_

This is a followup to https://github.com/astral-sh/uv/issues/7034 and https://github.com/astral-sh/uv/pull/7420.

It would be good to parse the scripting-related tables in `pyproject.toml`.

That unlocks two UX enhancements: 
 - `uv run` can key-match and warn in a selective and non-annoying way.
 - `uv sync` can directly display the script name (or the total number of script, if many) that it skipped.

---

_Referenced in [astral-sh/uv#7420](../../astral-sh/uv/pulls/7420.md) on 2024-09-16 14:22_

---

_Label `cli` added by @zanieb on 2024-09-16 14:23_

---
