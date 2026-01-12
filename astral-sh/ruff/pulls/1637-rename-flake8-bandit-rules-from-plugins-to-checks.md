```yaml
number: 1637
title: Rename flake8-bandit rules from plugins to checks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/checks
created_at: 2023-01-04T20:48:45Z
updated_at: 2023-01-04T20:49:06Z
url: https://github.com/astral-sh/ruff/pull/1637
synced_at: 2026-01-12T05:36:32Z
```

# Rename flake8-bandit rules from plugins to checks

---

_Pull request opened by @charliermarsh on 2023-01-04 20:48_

We may change this convention soon, since I think it's really unclear, but right now, functions that take a `Checker` are called "plugins", and pure functions that return a `Check` are called "checks".

---

_Merged by @charliermarsh on 2023-01-04 20:49_

---

_Closed by @charliermarsh on 2023-01-04 20:49_

---

_Branch deleted on 2023-01-04 20:49_

---
