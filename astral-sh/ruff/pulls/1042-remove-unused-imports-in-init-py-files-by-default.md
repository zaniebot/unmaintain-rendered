```yaml
number: 1042
title: "Remove unused imports in `__init__.py` files by default"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ignore-init-module-imports
created_at: 2022-12-04T19:22:52Z
updated_at: 2022-12-04T19:45:55Z
url: https://github.com/astral-sh/ruff/pull/1042
synced_at: 2026-01-12T15:55:05Z
```

# Remove unused imports in `__init__.py` files by default

---

_@charliermarsh_

This PR makes the current `F401` behavior (of flagging, but not removing unused imports in `__init__.py` files) opt-in, rather than enabled-for-all.

See: #484.


---

_Renamed from "Remove unused imports in __init__.py files by default" to "Remove unused imports in `__init__.py` files by default" by @charliermarsh on 2022-12-04 19:22_

---

_Merged by @charliermarsh on 2022-12-04 19:45_

---

_Closed by @charliermarsh on 2022-12-04 19:45_

---

_Branch deleted on 2022-12-04 19:45_

---
