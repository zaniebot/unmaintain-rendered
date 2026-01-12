```yaml
number: 2058
title: "Revert \"Upgrade to toml v0.5.11\""
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: revert-2040-charlie/toml
created_at: 2023-01-21T12:49:21Z
updated_at: 2023-01-21T12:54:58Z
url: https://github.com/astral-sh/ruff/pull/2058
synced_at: 2026-01-12T04:52:00Z
```

# Revert "Upgrade to toml v0.5.11"

---

_Pull request opened by @charliermarsh on 2023-01-21 12:49_

This _did_ fix https://github.com/charliermarsh/ruff/issues/1894, but was a little premature. `toml` doesn't actually depend on `toml-edit` yet, and `v0.5.11` was mostly about deprecations AFAICT. So upgrading might solve that issue, but could introduce other incompatibilities, and I'd like to minimize churn. I expect that `toml` will have a new release soon, so we can revert this revert.

Reverts charliermarsh/ruff#2040.


---

_Merged by @charliermarsh on 2023-01-21 12:54_

---

_Closed by @charliermarsh on 2023-01-21 12:54_

---

_Branch deleted on 2023-01-21 12:54_

---
