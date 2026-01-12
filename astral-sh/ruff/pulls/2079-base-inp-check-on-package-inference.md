```yaml
number: 2079
title: "Base `INP` check on package inference"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/inp
created_at: 2023-01-22T00:31:55Z
updated_at: 2023-01-22T00:49:57Z
url: https://github.com/astral-sh/ruff/pull/2079
synced_at: 2026-01-12T15:55:07Z
```

# Base `INP` check on package inference

---

_@charliermarsh_

If a file doesn't have a `package`, then it must both be in a directory that lacks an `__init__.py`, and a directory that _isn't_ marked as a namespace package.

Closes #2075.

---

_Merged by @charliermarsh on 2023-01-22 00:49_

---

_Closed by @charliermarsh on 2023-01-22 00:49_

---

_Branch deleted on 2023-01-22 00:49_

---
