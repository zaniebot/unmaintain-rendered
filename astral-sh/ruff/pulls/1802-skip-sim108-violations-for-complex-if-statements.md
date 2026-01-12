```yaml
number: 1802
title: Skip SIM108 violations for complex if-statements
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/SIM108
created_at: 2023-01-12T00:21:18Z
updated_at: 2023-01-12T00:21:31Z
url: https://github.com/astral-sh/ruff/pull/1802
synced_at: 2026-01-12T15:55:07Z
```

# Skip SIM108 violations for complex if-statements

---

_@charliermarsh_

We now skip SIM108 violations if: the resulting statement would exceed the user-specified line length, or the `if` statement contains comments.

Closes #1719.

Closes #1766.


---

_Merged by @charliermarsh on 2023-01-12 00:21_

---

_Closed by @charliermarsh on 2023-01-12 00:21_

---

_Branch deleted on 2023-01-12 00:21_

---
