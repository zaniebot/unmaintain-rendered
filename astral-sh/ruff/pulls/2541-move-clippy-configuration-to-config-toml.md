```yaml
number: 2541
title: Move Clippy configuration to config.toml
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/clippy
created_at: 2023-02-03T14:14:52Z
updated_at: 2023-02-03T14:26:38Z
url: https://github.com/astral-sh/ruff/pull/2541
synced_at: 2026-01-12T04:52:00Z
```

# Move Clippy configuration to config.toml

---

_Pull request opened by @charliermarsh on 2023-02-03 14:14_

This enables us to use a centralized configuration for the entire project, without having to add macros to each module or repeat precise commands everywhere. The main downside I've found is that modifying the configuration triggers a complete rebuild -- but that's a rare operation.

---

_Merged by @charliermarsh on 2023-02-03 14:26_

---

_Closed by @charliermarsh on 2023-02-03 14:26_

---

_Branch deleted on 2023-02-03 14:26_

---
