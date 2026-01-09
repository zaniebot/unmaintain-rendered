---
number: 12891
title: Should uv warn or error on overwriting a script in the venv?
type: issue
state: open
author: haarisr
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-04-15T06:42:53Z
updated_at: 2025-04-15T09:19:53Z
url: https://github.com/astral-sh/uv/issues/12891
synced_at: 2026-01-07T13:12:18-06:00
---

# Should uv warn or error on overwriting a script in the venv?

---

_Issue opened by @haarisr on 2025-04-15 06:42_

### Question

Hi,

I am generating scripts for different workspaces under the `[project.scripts]` section in `pyproject.toml` file.

If two seperate workspaces have the same script name, I would like uv to either warn or error when overwriting the script using `uv sync --all-packages`. There is no way to know which workspace emits the final script.

It would also be good to warn if overwriting a script provided by a 3rd-party tool as well.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @haarisr on 2025-04-15 06:42_

---

_Label `question` removed by @konstin on 2025-04-15 09:19_

---

_Label `enhancement` added by @konstin on 2025-04-15 09:19_

---

_Label `needs-decision` added by @konstin on 2025-04-15 09:19_

---
