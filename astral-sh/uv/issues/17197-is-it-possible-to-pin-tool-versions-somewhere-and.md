---
number: 17197
title: Is it possible to pin tool versions somewhere and install them later?
type: issue
state: closed
author: Andrej730
labels:
  - question
assignees: []
created_at: 2025-12-20T09:01:39Z
updated_at: 2025-12-20T12:05:35Z
url: https://github.com/astral-sh/uv/issues/17197
synced_at: 2026-01-10T01:26:14Z
---

# Is it possible to pin tool versions somewhere and install them later?

---

_Issue opened by @Andrej730 on 2025-12-20 09:01_

### Question

Hi! I was wondering if it's possible to list all my tools and their versions in some file - e.g. `requirements.txt` or as `project.optional-dependencies.dev-tools` in `pyproject.toml` and then use `uv tool install` in some way to install them from that file?

That would provide an easy way to install required system tools - can be used locally or can be used in CI for a project (typically in that case they're installed using `uv pip install .[dev]` or something, but in theory it can have conflicts with main project dependencies).


---

_Label `question` added by @Andrej730 on 2025-12-20 09:01_

---

_Comment by @zanieb on 2025-12-20 10:42_

Not yet, though we're interested. See https://github.com/astral-sh/uv/issues/12533

Note for project purposes, it's fine to have conflicts with the main dependencies. https://docs.astral.sh/uv/concepts/resolution/#conflicting-dependencies

---

_Comment by @Andrej730 on 2025-12-20 12:05_

Thanks, will subscribe to #12533.

---

_Closed by @Andrej730 on 2025-12-20 12:05_

---
