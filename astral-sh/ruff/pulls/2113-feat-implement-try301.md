```yaml
number: 2113
title: "feat: implement TRY301"
type: pull_request
state: merged
author: karpa4o4
labels: []
assignees: []
merged: true
base: main
head: feat/tryceratops-TRY301
created_at: 2023-01-23T19:55:51Z
updated_at: 2023-01-24T13:40:10Z
url: https://github.com/astral-sh/ruff/pull/2113
synced_at: 2026-01-12T04:52:00Z
```

# feat: implement TRY301

---

_Pull request opened by @karpa4o4 on 2023-01-23 19:55_

#2056

---

_@charliermarsh reviewed on 2023-01-23 23:46_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/raise_within_try.rs`:34 on 2023-01-23 23:46_

Should we avoid recursing if we hit another `try` block?

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/raise_within_try.rs`:34 on 2023-01-23 23:46_

Otherwise, we might raise the same error multiple times for nested `try` blocks.

---

_@charliermarsh reviewed on 2023-01-23 23:46_

---

_Review comment by @karpa4o4 on `src/rules/tryceratops/rules/raise_within_try.rs`:34 on 2023-01-24 10:24_

Done.

---

_@karpa4o4 reviewed on 2023-01-24 10:24_

---

_Merged by @charliermarsh on 2023-01-24 12:25_

---

_Closed by @charliermarsh on 2023-01-24 12:25_

---

_Branch deleted on 2023-01-24 13:40_

---
