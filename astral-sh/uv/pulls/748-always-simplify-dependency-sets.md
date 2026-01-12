```yaml
number: 748
title: Always simplify dependency sets
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2024-01-03T01:46:51Z
updated_at: 2024-01-03T18:21:05Z
url: https://github.com/astral-sh/uv/pull/748
synced_at: 2026-01-12T16:04:10Z
```

# Always simplify dependency sets

---

_@charliermarsh_

`simplify_set` can itself simplify to the full range, so it seems like we should be checking if the set is `Range::full` _after_ simplifying rather than before.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-03 01:46_

---

_@zanieb approved on 2024-01-03 17:21_

---

_Merged by @charliermarsh on 2024-01-03 18:21_

---

_Closed by @charliermarsh on 2024-01-03 18:21_

---

_Branch deleted on 2024-01-03 18:21_

---
