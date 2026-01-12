```yaml
number: 2266
title: Treat attribute constants as constant for yoda-conditions
type: pull_request
state: merged
author: frenck
labels: []
assignees: []
merged: true
base: main
head: frenck-2023-0225
created_at: 2023-01-27T17:07:47Z
updated_at: 2023-01-27T18:00:06Z
url: https://github.com/astral-sh/ruff/pull/2266
synced_at: 2026-01-12T04:52:00Z
```

# Treat attribute constants as constant for yoda-conditions

---

_Pull request opened by @frenck on 2023-01-27 17:07_

Accessed attributes that are Python constants should be considered for yoda-conditions


```py
## Error
JediOrder.YODA == age  # SIM300

## OK
age == JediOrder.YODA
```

~~PS: This PR will fail CI, as the `main` branch currently failing.~~

---

_Comment by @charliermarsh on 2023-01-27 17:43_

(Sorry, fixing `main`.)

---

_Comment by @frenck on 2023-01-27 17:50_

Rebased.

---

_Merged by @charliermarsh on 2023-01-27 17:55_

---

_Closed by @charliermarsh on 2023-01-27 17:55_

---

_Branch deleted on 2023-01-27 18:00_

---
