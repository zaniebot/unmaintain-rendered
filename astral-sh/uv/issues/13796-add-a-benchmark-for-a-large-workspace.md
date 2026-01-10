---
number: 13796
title: Add a benchmark for a large workspace
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2025-06-03T08:27:24Z
updated_at: 2025-06-03T08:27:24Z
url: https://github.com/astral-sh/uv/issues/13796
synced_at: 2026-01-10T01:25:38Z
---

# Add a benchmark for a large workspace

---

_Issue opened by @konstin on 2025-06-03 08:27_

To keep `uv run` usable in large projects, we need to keep the overhead low even if there are many projects in a workspace (see e.g. https://github.com/astral-sh/uv/pull/12096). This overhead may be parsing `pyproject.toml`s, parsing `uv.lock`, suboptimal directory traversal or quadratic freshness check. We should add a benchmark the performs warm cache `uv sync` and `uv run` in a large project such as airflow to continuously measure this overhead.

---

_Label `performance` added by @konstin on 2025-06-03 08:27_

---
