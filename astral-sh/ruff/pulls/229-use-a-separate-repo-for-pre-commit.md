```yaml
number: 229
title: Use a separate repo for pre-commit
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pre-commit
created_at: 2022-09-20T03:06:29Z
updated_at: 2022-09-20T03:06:40Z
url: https://github.com/astral-sh/ruff/pull/229
synced_at: 2026-01-12T15:55:04Z
```

# Use a separate repo for pre-commit

---

_@charliermarsh_

Resolves #225.

By defining the pre-commit configuration in _this_ repo, users had to build ruff from source, which requires Cargo. By putting the pre-commit configuration in a standalone repo that _depends_ on ruff, we can leverage PyPI's prebuilt wheels.


---

_Merged by @charliermarsh on 2022-09-20 03:06_

---

_Closed by @charliermarsh on 2022-09-20 03:06_

---

_Branch deleted on 2022-09-20 03:06_

---
