```yaml
number: 800
title: Ignore globals when checking local variable names
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ignore-globals
created_at: 2022-11-17T22:14:32Z
updated_at: 2022-11-17T22:19:02Z
url: https://github.com/astral-sh/ruff/pull/800
synced_at: 2026-01-12T15:55:05Z
```

# Ignore globals when checking local variable names

---

_@charliermarsh_

Resolves #799.

---

_Review comment by @charliermarsh on `src/check_ast.rs`:224 on 2022-11-17 22:18_

I don't see why we were setting the global in every scope. That feels wrong to me.

---

_@charliermarsh reviewed on 2022-11-17 22:18_

---

_Merged by @charliermarsh on 2022-11-17 22:19_

---

_Closed by @charliermarsh on 2022-11-17 22:19_

---

_Branch deleted on 2022-11-17 22:19_

---
