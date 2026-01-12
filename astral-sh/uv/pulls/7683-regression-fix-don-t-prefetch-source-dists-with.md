```yaml
number: 7683
title: "Regression fix: don't prefetch source dists with unbounded lower-bound ranges"
type: pull_request
state: merged
author: topherinternational
labels:
  - bug
assignees: []
merged: true
base: main
head: dont-prefetch-unbounded-lowerbound-packages
created_at: 2024-09-25T10:57:37Z
updated_at: 2024-09-25T12:56:46Z
url: https://github.com/astral-sh/uv/pull/7683
synced_at: 2026-01-12T16:07:56Z
```

# Regression fix: don't prefetch source dists with unbounded lower-bound ranges

---

_@topherinternational_

#7226 modified the check to skip prefetching of source dists without proper minimum-version bounds, and wound up flipping the boolean expression. This change flips the some/none expression so that the intended skip happens as expected.

Fixes #7680.


---

_@charliermarsh approved on 2024-09-25 12:35_

---

_Label `bug` added by @charliermarsh on 2024-09-25 12:35_

---

_Merged by @charliermarsh on 2024-09-25 12:35_

---

_Closed by @charliermarsh on 2024-09-25 12:35_

---

_Branch deleted on 2024-09-25 12:56_

---
