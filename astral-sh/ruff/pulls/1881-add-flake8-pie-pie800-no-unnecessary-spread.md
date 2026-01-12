```yaml
number: 1881
title: "Add flake8-pie PIE800: no-unnecessary-spread"
type: pull_request
state: merged
author: sbdchd
labels: []
assignees: []
merged: true
base: main
head: steve-flake8-pie800
created_at: 2023-01-15T02:57:54Z
updated_at: 2023-01-23T02:43:35Z
url: https://github.com/astral-sh/ruff/pull/1881
synced_at: 2026-01-12T04:51:59Z
```

# Add flake8-pie PIE800: no-unnecessary-spread

---

_Pull request opened by @sbdchd on 2023-01-15 02:57_

Checks for unnecessary spreads, like `{**foo, **{"bar": True}}`
rel: https://github.com/charliermarsh/ruff/issues/1879
rel: https://github.com/charliermarsh/ruff/issues/1543

---

_Comment by @charliermarsh on 2023-01-22 20:24_

This should be unblocked by the RustPython AST improvements that we just merged in.

---

_Marked ready for review by @sbdchd on 2023-01-22 23:28_

---

_Merged by @charliermarsh on 2023-01-23 02:43_

---

_Closed by @charliermarsh on 2023-01-23 02:43_

---
