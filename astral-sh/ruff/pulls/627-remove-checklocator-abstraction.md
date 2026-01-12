```yaml
number: 627
title: Remove CheckLocator abstraction
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/filter-rules
created_at: 2022-11-06T22:40:36Z
updated_at: 2022-11-06T22:42:11Z
url: https://github.com/astral-sh/ruff/pull/627
synced_at: 2026-01-12T05:48:45Z
```

# Remove CheckLocator abstraction

---

_Pull request opened by @charliermarsh on 2022-11-06 22:40_

Instead, just enforce that all checks are added via `add_check`, and modify the check location in the rare case that we're in an f-string.


---

_Merged by @charliermarsh on 2022-11-06 22:42_

---

_Closed by @charliermarsh on 2022-11-06 22:42_

---

_Branch deleted on 2022-11-06 22:42_

---
