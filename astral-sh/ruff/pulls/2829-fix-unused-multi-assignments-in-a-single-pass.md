```yaml
number: 2829
title: Fix unused multi-assignments in a single pass
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/once
created_at: 2023-02-12T22:25:12Z
updated_at: 2023-02-12T22:28:04Z
url: https://github.com/astral-sh/ruff/pull/2829
synced_at: 2026-01-12T15:55:11Z
```

# Fix unused multi-assignments in a single pass

---

_@charliermarsh_

This can leave behind a constant expression (`toplevel = tt = 1` gets fixed to `1`, rather than `pass`), but it's a lot more efficient and clearer.

---

_Merged by @charliermarsh on 2023-02-12 22:28_

---

_Closed by @charliermarsh on 2023-02-12 22:28_

---

_Branch deleted on 2023-02-12 22:28_

---
