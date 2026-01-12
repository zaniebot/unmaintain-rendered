```yaml
number: 1772
title: Disable doctests
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/doctests
created_at: 2023-01-10T19:47:50Z
updated_at: 2023-01-10T20:10:18Z
url: https://github.com/astral-sh/ruff/pull/1772
synced_at: 2026-01-12T15:55:07Z
```

# Disable doctests

---

_@charliermarsh_

We don't have any doctests, but `cargo test --all` spends more than half the time on doctests? A little confusing, but this brings the test time from > 4s to < 2s on my machine.


---

_Merged by @charliermarsh on 2023-01-10 20:10_

---

_Closed by @charliermarsh on 2023-01-10 20:10_

---

_Branch deleted on 2023-01-10 20:10_

---
