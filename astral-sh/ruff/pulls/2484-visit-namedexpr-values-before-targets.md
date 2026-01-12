```yaml
number: 2484
title: Visit NamedExpr values before targets
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/naned
created_at: 2023-02-02T17:17:58Z
updated_at: 2023-02-02T17:22:00Z
url: https://github.com/astral-sh/ruff/pull/2484
synced_at: 2026-01-12T04:52:00Z
```

# Visit NamedExpr values before targets

---

_Pull request opened by @charliermarsh on 2023-02-02 17:17_

Closes #2481.

---

_@charliermarsh reviewed on 2023-02-02 17:19_

---

_Review comment by @charliermarsh on `src/ast/visitor.rs`:270 on 2023-02-02 17:19_

This ensures that when we read `exponential` on the RHS, we treat it as a usage of `exponential` from the parent scope.

---

_Merged by @charliermarsh on 2023-02-02 17:21_

---

_Closed by @charliermarsh on 2023-02-02 17:21_

---

_Branch deleted on 2023-02-02 17:22_

---
