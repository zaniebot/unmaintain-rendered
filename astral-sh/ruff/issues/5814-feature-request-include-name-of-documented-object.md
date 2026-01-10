```yaml
number: 5814
title: "[feature-request] Include name of documented object in `D417` (`undocumented-param`) error message"
type: issue
state: closed
author: smackesey
labels:
  - good first issue
  - accepted
assignees: []
created_at: 2023-07-16T21:56:08Z
updated_at: 2023-07-17T02:51:35Z
url: https://github.com/astral-sh/ruff/issues/5814
synced_at: 2026-01-10T11:09:48Z
```

# [feature-request] Include name of documented object in `D417` (`undocumented-param`) error message

---

_Issue opened by @smackesey on 2023-07-16 21:56_

Just ran `D417` (`undocumented-param`) on the codebase. We haven't turned this on yet because we have so many violations. The errors printed look like this:

```
python_modules/dagster/dagster/_core/definitions/job_definition.py:587:9: D417 Missing argument descriptions in the docstring: `asset_selection`, `run_config`, `run_id`, `tags`
```

It's useful but it would be better if it included the names of the objects that the failing docstrings are attached to:

```
python_modules/dagster/dagster/_core/definitions/job_definition.py:587:9: D417 Missing argument descriptions in the docstring for `JobDefinition`: `asset_selection`, `run_config`, `run_id`, `tags`
```

---

_Comment by @charliermarsh on 2023-07-16 22:39_

Nice idea.

---

_Label `good first issue` added by @charliermarsh on 2023-07-16 22:39_

---

_Label `accepted` added by @charliermarsh on 2023-07-16 22:39_

---

_Comment by @charliermarsh on 2023-07-17 01:17_

(I wonder if we should just do this for all D rules?)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-17 02:15_

---

_Closed by @charliermarsh on 2023-07-17 02:51_

---
