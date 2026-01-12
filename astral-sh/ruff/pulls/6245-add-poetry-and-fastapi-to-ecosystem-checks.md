```yaml
number: 6245
title: Add Poetry and FastAPI to ecosystem checks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ecosystem
created_at: 2023-08-01T15:09:17Z
updated_at: 2023-08-01T15:48:35Z
url: https://github.com/astral-sh/ruff/pull/6245
synced_at: 2026-01-12T15:55:21Z
```

# Add Poetry and FastAPI to ecosystem checks

---

_@charliermarsh_

Poetry in particular would be useful to avoid issues like https://github.com/astral-sh/ruff/issues/6233.

---

_@charliermarsh reviewed on 2023-08-01 15:09_

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:123 on 2023-08-01 15:09_

I sorted this list, and added `Repository("tiangolo", "fastapi", "master")` and `Repository("python-poetry", "poetry", "master")`.

---

_Review requested from @zanieb by @charliermarsh on 2023-08-01 15:09_

---

_@zanieb approved on 2023-08-01 15:45_

---

_Merged by @charliermarsh on 2023-08-01 15:48_

---

_Closed by @charliermarsh on 2023-08-01 15:48_

---

_Branch deleted on 2023-08-01 15:48_

---
