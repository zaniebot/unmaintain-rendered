```yaml
number: 2095
title: Avoid nested-if violations when outer-if has else clause
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/else
created_at: 2023-01-22T22:38:32Z
updated_at: 2023-01-22T22:55:40Z
url: https://github.com/astral-sh/ruff/pull/2095
synced_at: 2026-01-12T15:55:07Z
```

# Avoid nested-if violations when outer-if has else clause

---

_@charliermarsh_

It looks like we need `do`-`while`-like semantics here with an additional outer check.

Closes #2094.

---

_Merged by @charliermarsh on 2023-01-22 22:40_

---

_Closed by @charliermarsh on 2023-01-22 22:40_

---

_Branch deleted on 2023-01-22 22:40_

---

_Comment by @harupy on 2023-01-22 22:55_

Thanks for the fix!

---
