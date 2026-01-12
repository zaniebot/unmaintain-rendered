```yaml
number: 7713
title: Update pyproject authors, maintainers, supported Python version labels
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/pyproject-maintenance
created_at: 2023-09-29T15:47:15Z
updated_at: 2023-09-29T17:04:12Z
url: https://github.com/astral-sh/ruff/pull/7713
synced_at: 2026-01-12T02:39:10Z
```

# Update pyproject authors, maintainers, supported Python version labels

---

_Pull request opened by @zanieb on 2023-09-29 15:47_

_No description provided._

---

_Label `internal` added by @zanieb on 2023-09-29 15:47_

---

_@charliermarsh reviewed on 2023-09-29 16:46_

---

_Review comment by @charliermarsh on `pyproject.toml`:33 on 2023-09-29 16:46_

Can you also update this in the README, where we say Python 3.11 compatible?

---

_@charliermarsh approved on 2023-09-29 16:46_

---

_@charliermarsh reviewed on 2023-09-29 16:48_

---

_Review comment by @charliermarsh on `pyproject.toml`:10 on 2023-09-29 16:48_

Should this be "Astral Software Inc."? I'm not sure. I see Prefect uses the full company name.

---

_@zanieb reviewed on 2023-09-29 16:53_

---

_Review comment by @zanieb on `pyproject.toml`:10 on 2023-09-29 16:53_

Sure Dagster uses "Dagster Labs" and Django uses "Django Software Foundation"

https://github.com/dagster-io/dagster/blob/master/python_modules/dagster/setup.py#L38
https://github.com/django/django/blob/main/setup.cfg#L5

---

_@zanieb reviewed on 2023-09-29 16:55_

---

_Review comment by @zanieb on `pyproject.toml`:10 on 2023-09-29 16:55_

Also https://discuss.python.org/t/the-author-maintainer-distinction-problem-and-pep-621/4562/21 should only maintain this once

---

_Review comment by @zanieb on `pyproject.toml`:10 on 2023-09-29 16:55_

```suggestion
authors = [{ name = "Astral Software Inc.", email = "hey@astral.sh" }]
```

---

_@zanieb reviewed on 2023-09-29 16:55_

---

_Merged by @zanieb on 2023-09-29 17:04_

---

_Closed by @zanieb on 2023-09-29 17:04_

---

_Branch deleted on 2023-09-29 17:04_

---
