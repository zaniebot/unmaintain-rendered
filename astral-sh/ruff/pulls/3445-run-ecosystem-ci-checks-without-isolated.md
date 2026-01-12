```yaml
number: 3445
title: Run ecosystem CI checks without --isolated
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isolated
created_at: 2023-03-10T23:11:53Z
updated_at: 2023-03-11T00:03:53Z
url: https://github.com/astral-sh/ruff/pull/3445
synced_at: 2026-01-12T04:39:44Z
```

# Run ecosystem CI checks without --isolated

---

_Pull request opened by @charliermarsh on 2023-03-10 23:11_

Using `--isolated` runs the risk that we _avoid_ respecting certain code paths. For example, if a project (like Airflow) uses namespace packages, we'll no longer respect those settings, and we run the risk of introducing regressions for namespace packages.


---

_Comment by @github-actions[bot] on 2023-03-10 23:22_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Merged by @charliermarsh on 2023-03-11 00:03_

---

_Closed by @charliermarsh on 2023-03-11 00:03_

---

_Branch deleted on 2023-03-11 00:03_

---
