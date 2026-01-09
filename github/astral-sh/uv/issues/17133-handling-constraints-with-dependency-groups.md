---
number: 17133
title: handling constraints with dependency groups
type: issue
state: open
author: karanr-invent
labels:
  - question
assignees: []
created_at: 2025-12-15T15:10:52Z
updated_at: 2025-12-16T13:43:38Z
url: https://github.com/astral-sh/uv/issues/17133
synced_at: 2026-01-07T13:12:19-06:00
---

# handling constraints with dependency groups

---

_Issue opened by @karanr-invent on 2025-12-15 15:10_

### Question

Hi,
First of all a big thanks for all the amazing work :-)
We have a scenario where we have airflow and spark env, to manage the same we are using dependency groups in single `pyproject.toml`. All the common packages are part of dependencies and rest belong to their respective groups.
Now to setup env, we create multiple env and run
`uv sync --group airflow`
this way we setup our env's.
Now my question is as airflow usually have constraints and rather a long list, how can we sync any specific venv based on dependency group with constraints and not the other.
Let me know if u need more clarity, will be happy to share.
Thanks

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @karanr-invent on 2025-12-15 15:10_

---

_Comment by @konstin on 2025-12-16 13:43_

uv builds a lockfile using all dependencies and all constraints together, so currently the only option would be to apply the constraints to the entire lockfile.

---
