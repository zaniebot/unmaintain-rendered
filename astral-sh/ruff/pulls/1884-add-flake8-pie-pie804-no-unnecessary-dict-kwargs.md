```yaml
number: 1884
title: "Add flake8-pie PIE804: no-unnecessary-dict-kwargs"
type: pull_request
state: merged
author: sbdchd
labels: []
assignees: []
merged: true
base: main
head: steve-pie804
created_at: 2023-01-15T04:25:26Z
updated_at: 2023-01-23T02:32:45Z
url: https://github.com/astral-sh/ruff/pull/1884
synced_at: 2026-01-12T15:55:07Z
```

# Add flake8-pie PIE804: no-unnecessary-dict-kwargs

---

_@sbdchd_

Warn about things like `foo(**{"bar": True})` which is equivalent to `foo(bar=True)`

rel: https://github.com/charliermarsh/ruff/issues/1879
rel: https://github.com/charliermarsh/ruff/issues/1543

---

_Comment by @charliermarsh on 2023-01-22 20:24_

This should be unblocked by the RustPython AST improvements that we just merged in.

---

_Renamed from "wip: Add flake8-pie PIE804: no-unnecessary-dict-kwargs" to "Add flake8-pie PIE804: no-unnecessary-dict-kwargs" by @sbdchd on 2023-01-22 23:32_

---

_Marked ready for review by @sbdchd on 2023-01-23 00:04_

---

_Merged by @charliermarsh on 2023-01-23 02:32_

---

_Closed by @charliermarsh on 2023-01-23 02:32_

---
