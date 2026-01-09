---
number: 16996
title: "`--frozen` causes `uv sync` to silently ignore resolution options"
type: issue
state: open
author: konstin
labels: []
assignees: []
created_at: 2025-12-05T09:27:08Z
updated_at: 2025-12-05T09:27:08Z
url: https://github.com/astral-sh/uv/issues/16996
synced_at: 2026-01-07T13:12:19-06:00
---

# `--frozen` causes `uv sync` to silently ignore resolution options

---

_Issue opened by @konstin on 2025-12-05 09:27_

When running `uv sync` and then `uv sync --resolution lowest-direct --fork-strategy fewest --prerelease allow --exclude-newer 2025-01-01 --frozen`, uv silently ignores the resolution options due to `--frozen`, even though the user requested something that uv can't fulfill (a specific type of resolution with `--frozen`).

---
